KPCLI-2.8(1)          User Contributed Perl Documentation         KPCLI-2.8(1)



NAME
       kpcli - A command line interface to KeePass database files.

DESCRIPTION
       A command line interface (interactive shell) to work with KeePass
       database files (http://http://en.wikipedia.org/wiki/KeePass).  This
       program was inspired by my use of "kedpm -c" combined with my need to
       migrate to KeePass. The curious can read about the Ked Password Manager
       at http://kedpm.sourceforge.net/.

USAGE
       Please run the program and type "help" to learn how to use it.

PREREQUISITES
       This program requires these non-core modules:

       "Crypt::Rijndael" - libcrypt-rijndael-perl on Ubuntu 10.04

       "Term::ReadKey"   - libterm-readkey-perl on Ubuntu 10.04

       "Sort::Naturally" - libsort-naturally-perl on Ubuntu 10.04

       "File::KeePass"   - libfile-keepass-perl on Ubuntu 12.04

       "Term::ShellUI"   - libterm-shellui-perl on Ubuntu 12.10

       It is recommended that you install "Term::ReadLine::Gnu" which will
       provide more fluid signal handling on Unix-like systems, making kpcli
       robust to suspend, resume, and interupt - SIGSTP, SIGCONT and SIGINT.
       That module is in the libterm-readline-gnu-perl package on Ubuntu.

       You can optionally install "Clipboard" and "Tiny::Capture" to use the
       clipboard features; http://search.cpan.org/~king/Clipboard/ and
       libcapture-tiny-perl on Ubuntu 10.04.

       You can optionally install "Data::Password" to use the pwck feature
       (Password Quality Check); libdata-password-perl on Ubuntu 10.04.

       On MS Windows, you can optionally install "Win32::Console::ANSI" to get
       ANSI colors in Windows cmd terminals. Strawberry Perl 5.16.2 was used
       for the kpcli port to Windows and, using cpanminus, one can install all
       of kpcli's dependencies, sans Term::ReadLine::Gnu which is optional for
       kpcli and not supported on MS Windows.

CAVEATS AND WORDS OF CAUTION
       The main author of kpcli primarily interoperability tests with KeePassX
       (http://www.keepassx.org/) and primarily uses KeePass v1 (*.kdb) files.
       Support for KeePass v2 (*.kdbx) files in kpcli is substantial, and many
       people use it daily, but it is not the author's primary use case. It is
       also the author's intent to maintain compatibility with v1 files, and
       so anyone sending patches, for consideration for inclusion in future
       kpcli versions, is asked to validate them with both v1 and v2 files.

   No history tracking for KeePass 2 (*.kdbx) files
       Recording entries' history in KeePass 2 files is not implemented.
       History that exists in a file is not destroyed, but results of entry
       changes made in kpcli are not recorded into their history. Prior-to-
       change copies are stored into the "Recycle Bin." Note that
       File::KeePass does not encrypt passwords of history entries in RAM,
       like it does for current entries.  This is a small security risk that
       can, in theory, allow priviledged users to steal your passwords from
       RAM, from entry history.

   File::KeePass bug prior to version 2.03
       Prior to version 2.03, File::KeePass had a bug related to some
       "unknown" data that KeePassX stores in group records. For File::KeePass
       < v2.03, kpcli deletes those unknown data when saving. Research in the
       libkpass (http://libkpass.sourceforge.net/) source code revealed that
       what early versions of File::KeePass classifies as "unknown" are the
       times for created/modified/accessed/expires as well as "flags" (id=9),
       but only for groups; File::KeePass handled those fields just fine for
       entries.  I found no ill-effect from dropping those fields when saving
       and so that is what kpcli does to work around the File::KeePass bug, if
       kpcli is using File::KeePass < v2.03.

BUGS
   Using Ctrl-D to Exit
       Versions of Term::ShellUI prior to v0.9. do not have the ability to
       trap Ctrl-D exits by the client program. I submitted a patch to remedy
       that and it made it into Term::ShellUI v0.9. Please upgrade if kpcli
       asks you to.

   Multiple Entries or Groups With the Same Name in the Same Group
       This program does not support multiple entries in the same group having
       the exact same name, nor does it support multiple groups at the same
       level having the same name, and it likely never will. KeepassX does
       support those.  This program detects and alert when an opened database
       file has those issues, but it does not refuse to save (overwrite) a
       file that is opened like that. Saves are actually safe (no data loss)
       as long as the user has not touched one of the duplicately-named items.

AUTHOR
       Lester Hightower <hightowe at cpan dot org>

LICENSE
       This program may be distributed under the same terms as Perl itself.

CREDITS
       Special thanks to Paul Seamons, author of "File::KeePass", and to Scott
       Bronson, author of "Term::ShellUI". Without those two modules this
       program would not have been practical for me to author.

CHANGELOG
        2010-Nov-28 v0.1 - Initial release.
        2010-Nov-28 v0.2 - Encrypt the master password in RAM.
        2010-Nov-29 v0.3 - Fixed master password encryption for saveas.
        2010-Nov-29 v0.4 - Fixed code to work w/out Term::ReadLine::Gnu.
                           Documented File::KeePass v0.1 hierarchy bug.
        2010-Nov-29 v0.5 - Made find command case insensitive.
                           Bugfix in new command (path regex problem).
        2010-Nov-29 v0.6 - Added lock file support; warn if a lock exists.
        2010-Dec-01 v0.7 - Further documented the group fields that are
                            dropped, in the CAVEATS section of the POD.
                           Sort group and entry titles naturally.
        2010-Dec-23 v0.8 - Worked with File::KeePass author to fix a couple
                            of bugs and then required >=v0.03 of that module.
                           Sorted "/_found" to last in the root group list.
                           Fixed a "database changed" state bug in cli_save().
                           Made the find command ignore entries in /Backup/.
                           Find now offers show when only one entry is found.
                           Provided a patch to Term::ShellUI author to add
                            eof_exit_hook and added support for it to kpcli.
        2011-Feb-19 v0.9 - Fixed bugs related to spaces in group names as
                            reported in SourceForge bug number 3132258.
                           The edit command now prompts to save on changes.
                           Put scrub_unknown_values_from_all_groups() calls
                            back into place after realizing that v0.03 of
                           File::KeePass did not resolve all of the problems.
        2011-Apr-23 v1.0 - Changed a perl 5.10+ regex to a backward-compatable
                            one to resolve SourceForge bug number 3192413.
                           Modified the way that the /Backup group is ignored
                            by the find command to stop kpcli from croaking on
                            multiple entries with the same name in that group.
                            - Note: There is a more general bug here that
                                    needs addressing (see BUGS section).
                           An empty title on new entry aborts the new entry.
                           Changed kdb files are now detected/warned about.
                           Tested against Term::ShellUI v0.9, which has my EOF
                            hook patch, and updated kpcli comments about it.
                           Term::ShellUI's complete_history() method was
                            removed between v0.86 and v0.9 and so I removed
                            kpli's call to it (Ctrl-r works for history).
                           Added the "icons" command.
        2011-Sep-07 v1.1 - Empty DBs are now initialized to KeePassX style.
                           Fixed a couple of bugs in the find command.
                           Fixed a password noecho bug in the saveas command.
                           Fixed a kdb_has_changed bug in the saveas command.
                           Fixed a cli_open bug where it wasn't cli_close'ing.
                           Fixed variable init bugs in put_master_passwd().
                           Fixed a false warning in warn_if_file_changed().
        2011-Sep-30 v1.2 - Added the "export" command.
                           Added the "import" command.
                           Command "rmdir" asks then deletes non-empty groups.
                           Command "new" can auto-generate random passwords.
        2012-Mar-03 v1.3 - Fixed bug in cl command as reported in SourceForge
                            bug number 3496544.
        2012-Apr-17 v1.4 - Added key file support based on a user contributed
                            patch with SourceForge ID# 3518388.
                           Added my_help_call() to allow for longer and more
                            descriptive command summaries (for help command).
                           Stopped allowing empty passwords for export.
        2012-Oct-13 v1.5 - Fixed "help <foo>" commands, that I broke in v1.4.
                           Command "edit" can auto-generate random passwords.
                           Added the "cls" and "clear" commands from a patch
                            with SourceForge ID# 3573930.
                           Tested compatibility with File::KeePass v2.03 and
                            made minor changes that are possible with >=2.01.
                           With File::KeePass v2.03, kpcli should now support
                            KeePass v2 files (*.kdbx).
        2012-Nov-25 v1.6 - Hide passwords (red on red) in the show command
                            unless the -f option is given.
                           Added the --readonly command line option.
                           Added support for multi-line notes/comments;
                            input ends on a line holding a single ".".
        2013-Apr-25 v1.7 - Patched to use native File::KeePass support for key
                            files, if the File::KeePass version is new enough.
                           Added the "version" and "ver" commands.
                           Updated documentation as Ubuntu 12.10 now packages
                            all of kpcli's dependencies.
                           Added --histfile command line option.
                           Record modified times on edited records, from a
                            patch with SourceForge ID# 3611713.
                           Added the -a option to the show command.
        2013-Jun-09 v2.0 - Removed the unused Clone module after a report that
                            Clone is no longer in core Perl as of v5.18.0.
                           Added the stats and pwck commands.
                           Added clipboard commands (xw/xu/xp/xx).
                           Fixed some long-standing tab completion bugs.
                           Warn if multiple groups or entries are titled the
                            same within a group, except for /Backup entries.
        2013-Jun-10 v2.1 - Fixed several more tab completion bugs, and they
                            were serious enough to warrant a quick release.
        2013-Jun-16 v2.2 - Trap and handle SIGINT (^C presses).
                           Trap and handle SIGTSTP (^Z presses).
                           Trap and handle SIGCONT (continues after ^Z).
                           Stopped printing found dictionary words in pwck.
        2013-Jul-01 v2.3 - More readline() and signal handling improvements.
                           Title conflict checks in cli_new()/edit()/mv().
                           Group title conflict checks in rename().
                           cli_new() now accepts optional path&|title param.
                           cli_ls() can now list multiple paths.
                           cli_edit() now shows the "old" values for users
                            to edit, if Term::ReadLine::Gnu is available.
                           cli_edit() now aborts all changes on ^C.
                           cli_saveas() now asks before overwriting a file.
        2013-Nov-26 v2.4 - Fixed several "perl -cw" warnings reported on
                            2013-07-09 as SourceForge bug #9.
                           Bug fix for the cl command, but in sub cli_ls().
                           First pass at Strawberry perl/MS Windows support.
                            - Enhanced support for Term::ReadLine::Perl
                            - Added support for Term::ReadLine::Perl5
                           Added display of expire time for show -a.
                           Added -a option to the find command.
                           Used the new magic_file_type() in a few places.
                           Added generatePasswordFromDict() and "w" generation.
                           Added the -v option to the version command.
                            - Added the versions command.
        2014-Mar-15 v2.5 - Added length control (gNN) to password generation.
                           Added the copy command (and cp alias).
                           Added the clone command.
                           Added optional modules not installed to version -v.
                           Groups can now also be moved with the mv command.
                           Modified cli_cls() to also work on MS Windows.
                           Suppressed Term::ReadLine::Gnu hint on MS Windows.
                           Suppressed missing termcap warning on MS Windows.
                           Print a min number of *s to not leak passwd length.
                           Removed unneeded use of Term::ReadLine.
                           Quieted "inherited AUTOLOAD for non-method" warns
                            caused by Term::Readline::Gnu on perl 5.14.x.
        2014-Jun-06 v2.6 - Added interactive password generation ("i" method).
                            - Thanks to Florian Tham for the idea and patch.
                           Show entry's tags if present (KeePass >= v2.11).
                            - Thanks to Florian Tham for the patch.
                           Add/edit support for tags if a v2 file is opened.
                           Added tags to the searched fields for "find -a".
                           Show string fields (key/val pairs) in v2 files.
                           Add/edit for string fields if a v2 file is opened.
                           Show information about entries' file attachments.
                            2014-03-20 SourceForge feature request #6.
                           New "attach" command to manage file attachments.
                           Added "Recycle Bin" functionality and --no-recycle.
                           For --readonly, don't create a lock file and don't
                            warn if one exists. 2014-03-27 SourceForge bug #11.
                           Added key file generation to saveas and export.
                            2014-04-19 SourceForge bug #13.
                           Added -expired option to the find command.
                           Added "dir" as an alias for "ls"
                           Added some additional info to the stats command.
                           Added more detailed OS info for Linux/Win in vers.
                           Now hides Meta-Info/SYSTEM entries.
                           Fixed bug with SIGTSTP handling (^Z presses).
                           Fixed missing refresh_state_all_paths() in cli_rm.
        2014-Jun-11 v2.7 - Bug fix release. Broke the open command in 2.6.
        2015-Feb-08 v2.8 - Fixed cli_copy bug; refresh paths and ask to save.
                           Fixed a cli_mv bug; double path-normalization.
                           Fixed a path display bug, if done after a cli_mv.
                           Protect users from editing in the $FOUND_DIR.
                           Keep file opened, read-only, to show up in lsof.
                           Added inactivity locking (--timeout parameter).
                           Added shell expansion support to cli_ls, with the
                            ability to manage _all_ listed entries by number.
                           Added shell expansion support to cli_mv.
                           Added [y/N] option to list entries after a find.

TODO ITEMS
         Consider broadening shell_expansion support beyond just mv and ls.

         Consider adding a purge command for "Backup"/"Recycle Bin" folders.

         Consider adding a tags command for use with v2 files.
          - To navigate by entry tags

         Consider supporting KeePass 2.x style entry history.
          - There are potential security implications in File::KeePass.
          - Related, consider adding a purge command for that history.

         Consider adding KeePass 2.x style multi-user synchronization.

         Consider http://search.cpan.org/~sherwin/Data-Password-passwdqc/
         for password quality checking.

         Consider adding searches for created, modified, and accessed times
         older than a user supplied time.

         Consider adding import support for Password Safe v3 files using
         http://search.cpan.org/~tlinden/Crypt-PWSafe3/. May have already
         done this if the unpackaged dependencies list was not so long.

OPERATING SYSTEMS AND SCRIPT CATEGORIZATION
       Unix-like
        - Originally written and tested on Ubuntu Linux 10.04.1 LTS.
        - As of version 2.5, development is done on Linux Mint 16.
        - Known to work on many other Linux and *BSD distributions, and
          kpcli is packaged with many distributions now-a-days.

       Microsoft Windows
        - As of v2.4, Microsoft Windows is also supported.
        - Tested and compiled on Strawberry Perl 5.16.2 on Windows 7.

       "UNIX/System_administration", "Win32/Utilities"



perl v5.14.2                      2015-02-08                      KPCLI-2.8(1)
