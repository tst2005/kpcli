# my TODO list

## stdin support for commands

* Be able to use stdin to send commands. Something like `printf 'help\nversion\nexit\n' | ./kpcli.pl`
  Not planned to be used for providing the password. See password option.

See the `new(NAME,[IN[,OUT]])` in the [Term-ReadLine-Gnu doc](http://search.cpan.org/~hayashi/Term-ReadLine-Gnu-1.26/Gnu.pm#Standard_Methods)

## xml export

* Implement an plain xml export (not only encrypted .kdb format)

## ls -l

* Improve the kpcli `ls` command to support option (like `ls -l` even to just ignore the option ... but do the action anyway)

## password option

* Add option (--no-passwd ?) to support open db only with key file (seems supported by the File::KeePass lib, see [load_db](https://metacpan.org/pod/File::KeePass#load_db))
* Add way to provide the password over option (yes it's less secure) but mandatory for automatic script stuff...

## security actions over script

See if it's possible to (just a sample):
* open a kdb database with password and key
* save it in a new kdb file without password, without key
* close the secure one

* open the not encrypted one
* do stuff (add/delete/change.../import/...)
* save changes

* save in another new secure kdb, with new password and key


Possible improvement :
* more option about password (keepassx and kpcli currently use (only?) the "Composite" way)

