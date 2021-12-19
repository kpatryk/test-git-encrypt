[![GitHub Actions Demo](https://github.com/kpatryk/test-git-encrypt/actions/workflows/github.yml/badge.svg)](https://github.com/kpatryk/test-git-encrypt/actions/workflows/github.yml)

Test encryption of the repository content.

# Useful commands

1. Encrypt a file with GPG:

        gpg --symmetric --cipher-algo AES256 input.json

    A new file will be created with `gpg` extension.

2. Decrypt a file:

        gpg --quiet --batch --yes --decrypt --passphrase="$SECRET_PASSPHRASE" \
        --output output.json input.json.gpg


# Git crypt usage

## Setup repository to work with Git-Crypt


1. Install Git-Crypt on Mac OS

        brew install git-crypt

2. Init a new key in a fresh new repository:

        mkdir <new-repo>
        cd <new-repo>
        git init
        echo "sample file" > sample.txt
        git add -A
        git commit -m "Initial commit"
        git-crypt init

3. Save your newly generated key:

        git-crypt export <path-to-filename>

4. Add `.gitattributes` to inform Git-Crypt what files should be encrypted:

        echo "api.key filter=git-crypt diff=git-crypt" > .gitattributes
        echo "mySuperSecretAPI_Key" > api.key
        git add api.key
        git commit -m "Add API key"

5. Check status of ALL files in the repository:

        git-crypt status    #Show status of all files
        git-crypt status -e #Show encrypted files only
        git-crypt status -u #Show unencrypted files only

## Export Git-Crypt key

1. Export git-crypt key to a custom path:

        git-crypt export <path-to-key>

## Verification that encryption-decryption is working as expected

1. Check the content of the added file:

        file api.key

    Expected result:

        api.key: ASCII text

2. Lock the files in the current repository by encrypting them.

        git-crypt lock

3. Check status of the files:

        file api-key

4. Unlock files:

        git-crypt unlock <path>
        file api.key

    Expected result:

        api.key: data

## Re-use the private key generated previously in the other repository

    mkdir <other-repo>
    cd <other-repo>
    git init
    echo "Repo 2" > repo.txt
    git add repo.txt
    git commit -m "Initial commit"
    git-crypt unlock <path-to-key>

# Important Notes
- Make sure you export your encryption key to a safe place, as it is required to be able to decrypt the files after cloning the repository
- Losing the key won't allow you to decrypt the content of the repository encrypted files!

# Documentation
- https://buddy.works/guides/git-crypt
- https://hackernoon.com/things-you-must-know-about-git-crypt-to-successfully-protect-your-secret-data-kyi3wi6
