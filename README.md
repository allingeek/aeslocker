# aeslocker

A tool that provides file encryption operations, "lock" and "unlock." Files are encrypted using AES256 CBC and written in base64. This tool does not currently use any MAC for integrity checking.

## Dependencies

The current implementation depegates to OpenSSL as shipped on OSX.

## Locking a file

Copy aeslocker to a location in your PATH and run it with the `lock` subcommand followed by the paths of target files. For example, someone wanting to encrypt their private key (id_rsa) might run:

    aeslocker lock ~/.ssh/id_rsa

The user will be prompted for the passphrase on STDIN and again for validation. Upon successful locking the original file will be deleted and the encrypted file will be written to the same location with the suffix, `.enc`.

## Locking a BUNCH of files

Sometimes you want to lock a bunch of files... Well the lock and unlock subcommands both take a variable number of arguments which it interpret as files.

    $ aeslocker lock ~/secrets/fileA ~/secrets/fileB ~/secrets/fileC ~/secrets/fileD ~/otherSecrets/someOtherfile
    Locking ~/secrets/fileA
    enter aes-256-cbc encryption password:
    Verifying - enter aes-256-cbc encryption password:
    Locking ~/secrets/fileB
    enter aes-256-cbc encryption password:
    Verifying - enter aes-256-cbc encryption password:
    Locking ~/secrets/fileC
    enter aes-256-cbc encryption password:
    Verifying - enter aes-256-cbc encryption password:
    Locking ~/secrets/fileD
    enter aes-256-cbc encryption password:
    Verifying - enter aes-256-cbc encryption password:
    Locking ~/otherSecrets/someOtherfile
    enter aes-256-cbc encryption password:
    Verifying - enter aes-256-cbc encryption password:

## Unlocking a file

With aeslocker on your PATH, run it with the `unlock` subcommand followed by the paths of target files. **Do not include the `.enc` file suffix.** For example, someone wanting to encrypt their private key (id_rsa) might run:

    aeslocker lock ~/.ssh/id_rsa

The user will be prompted for the passphrase on STDIN. Upon successful unlocking the original file will be deleted.

## For the love of composition...

Unlock all of the files in the current working tree:

    aeslocker unlock $(find . -name '*.enc' -print | sed -n 's/^\(.*\)\.enc$/\1/p')

