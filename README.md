<p align="center">
  <img src="https://github.com/jotavare/jotavare/blob/main/42/banners/piscine_and_common_core/github_piscine_and_common_core_banner_minishell.png">
</p>

<p align="center">
	<img src="https://img.shields.io/badge/status-finished-success?color=%2312bab9&style=flat-square"/>
	<img src="https://img.shields.io/badge/evaluated-18%20%2F%2012%20%2F%202022-success?color=%2312bab9&style=flat-square"/>
	<img src="https://img.shields.io/badge/score-100%20%2F%20100-success?color=%2312bab9&style=flat-square"/>
	<img src="https://img.shields.io/github/languages/top/jotavare/minishell?color=%2312bab9&style=flat-square"/>
	<img src="https://img.shields.io/github/last-commit/jotavare/minishell?color=%2312bab9&style=flat-square"/>
	<a href='https://www.linkedin.com/in/joaoptoliveira' target="_blank"><img alt='Linkedin' src='https://img.shields.io/badge/LinkedIn-100000?style=flat-square&logo=Linkedin&logoColor=white&labelColor=0A66C2&color=0A66C2'/></a>
	<a href='https://profile.intra.42.fr/users/jotavare' target="_blank"><img alt='42' src='https://img.shields.io/badge/Porto-100000?style=flat-square&logo=42&logoColor=white&labelColor=000000&color=000000'/></a>
</p>

<p align="center">
	<a href="#before-evaluation">Before evaluation</a> •
	<a href="#utilities">Utilities</a> •
	<a href="#norminette">Norminette</a> •
	<a href="#license">License</a>
</p>

## ABOUT
The existence of shells is linked to the very existence of IT. At the time, all developers agreed that communicating with a computer using aligned 1/0 switches was seriously irritating. It was only logical that they came up with the idea of creating a software to communicate with a computer using interactive lines of commands in a language somewhat close to the human language.

Thanks to Minishell, I’ll be able to travel through time and come back to problems people faced when Windows didn’t exist. This project is about creating a simple shell, my own little bash. I learned a lot about processes and file descriptors.

- [Subject](https://github.com/jotavare/minishell/blob/main/subject/en_subject_minishell.pdf) `PDF`
- [References](https://github.com/jotavare/42-resources#03-minishell) `GitHub`

## BEFORE EVALUATION
- [ ] Check norminette for any errors.
- [ ] Check for segmentation faults, bus errors, double free, leaks, ...
- [ ] Must compile with -Wall, -Wextra and -Werror.
- [ ] Makefile must contain $(NAME), all, clean, fclean.
- [ ] Check for forbidden functions in your code.
- [ ] Check the [evaluation page](https://github.com/jotavare/jotavare/blob/main/42/project_evaluations/minishell_evaluation_page.pdf) for any errors, leaks or other requirements.

| Important Commands                | Description                                                              |
| :-------------------------------- | :----------------------------------------------------------------------- |
| `make -n`                         | Display the compilation information without actually compiling the code. |
| `echo $?`                         | Display the exit status of the last executed command.                    |
| `nm -g ./minishell \| grep " U "` | Check for forbidden functions.                                           |
| `norminette`                      | Checks the code for compliance with the coding style and guidelines.     |
| `-R CheckForbiddenSourceHeader`   | Check the code in header files.                                          |
| `cc -g -Wall -Wextra -Werror`     | Compile with the mandatory flags.                                        |
| `-fsanitize=address`              | Check for leaks.                                                         |
| `-lreadline`                      | Necessary to use the readline library.                                   |
| `ps -e` or `ps -A`                | See all processes currently running on your computer.                    |
| `ps -a`                           | Check for any dead children.                                             |

| Valgrind Flags                 | Description                                      |
| :----------------------------- | :----------------------------------------------- |
| `valgrind ./minishell`         | Check for leaks                                  |
| `-leak-check=full`             | Detailed checking for memory leaks.              |
| `--show-leak-kinds=all`        | Display all types of memory leaks.               |
| `--track-origins=yes`          | Tracks the origins of uninitialized values.      |
| `--verbose`                    | Increases the level of verbosity.                |
| `--gen-suppressions=all`       | Ignore specific known issues or false positives. |
| `--suppressions=readline.supp` | Specifies the path to a suppression file.        |
| `--log-file=memleaks.log`      | Sets the name of the file.                       |

> To ignore leaks related to the `readline` and `add_history` functions, create a file called `readline.supp` with the following content:
```
{
    leak readline
    Memcheck:Leak
    ...
    fun:readline
}
{
    leak add_history
    Memcheck:Leak
    ...
    fun:add_history
}
```

`valgrind --suppressions=readline.supp --leak-check=full --show-leak-kinds=all ./minishell`

## UTILITIES
### Return Values ($?)

* All Linux commands return an error code between `0` and `255`.
* The value 0 represents the value true (command success).
* Values greater than 0 represent false (command failure).
* The error code of the last command used is contained in the variable `$?`.

| $?      | Description                                                                      |
| :------ | :------------------------------------------------------------------------------- |
| `1`     | Standard for general errors, such as a division by zero.                         |
| `2`     | Improper use of built-in commands, per Bash documentation.                       |
| `126`   | The command called cannot be executed, rights problem or command not executable. |
| `127`   | Command not found, possible problem with $PATH or typing error.                  |
| `128`   | Invalid command argument.                                                        |
| `128+n` | 128 + signal number.                                                             |
| `130`   | Finished with `Ctrl` + `C` (130 = 128 + 2).                                      |
| `255`   | Exit code out of bounds. Example: `exit -1`.                                     |

### Command Reminders
| Command       | Description                                                                                 |
| :------------ | :------------------------------------------------------------------------------------------ |
| `yes`		| Writes in an infinite loop `yes teste`.                                                     |
| `ln`		| Bind a file or directory.                                                                   |
| `chmod`	| Change file permissions `chmod 777` (all permissions) `chmod 000` (no permissions).         |
| `cd`		| Change directory. `cd -` (last visited directory) `cd` (user directory) `cd /` (root).      |
| `clear`	| Clear the screen.                                                                           |	
| `diff`	| Compare files line by line.                                                                 |
| `cmp`		| Write the first line of difference between 2 files.                                         |
| `pc`		| Copying files.                                                                              |
| `rm`		| Delete file.                                                                                |
| `rm -rf`	| Delete the directory recursively.                                                           |
| `ls -l`	| Show the contents of the directory.                                                         |
| `exit`	| Exit current process.                                                                       |
| `grep`	| Search for strings in files `grep "printf" file`.                                           |
| `mkdir`	| Create a directory.                                                                         |
| `rmdir`	| Delete a directory.                                                                         |
| `more`	| Displays a file page by page as in a man.                                                   |
| `mv`		| Move or rename.                                                                             |
| `$PATH`	| Path to executables.                                                                        |
| `cat`		| Send the file to stdout.                                                                    |

### CHMOD
| Rights        | Number |
| :------------ | :----- |
| `r` (read)	| `4`    |
| `w` (write)	| `2`    |
| `x` (execute)	| `1`    |
	
| Rights  | Calculation | Total |
| :------ | :---------- | :---- |
| `---`   | `0+0+0`     | `0`   |
| `r--`   | `4+0+0`     | `4`   |
| `-w-`   | `0+2+0`     | `2`   |
| `--x`   | `0+0+1`     | `1`   |
| `rw-`   | `4+2+0`     | `6`   |
| `-wx`   | `0+2+1`     | `3`   |
| `x-ray` | `4+0+1`     | `5`   |
| `rwx`   | `4+2+1`     | `7`   |

### MAX/MIN
| Data Types | Qualifiers                        | Size (in byte) | Range                          |
| :--------- | :-------------------------------- | :------------- | :----------------------------- |
| `char`     | `char` or `signed char`		 | `1`            | `-128` to `127`                |
| `char`     | `unsigned char`			 | `1`            | `0` to `255`                   |
| `int`	     | `int` or `signed int`		 | `4`            | `-2147483648` to `2147483647`  |
| `int`	     | `unsigned int`			 | `4`            | `0` to `4294967295`            |
| `int`	     | `short int` or `short signed int` | `2`            | `-32768` to `32767`            |
| `int`	     | `unsigned short int`		 | `2`            | `0` to `65535`                 |
| `int`	     | `long int` or `signed long int`	 | `4`            | `-2147483648` to `2147483647`  |
| `int`	     | `unsigned long int`		 | `4`            | `0` to `4294967295`            |
| `float`    | `float`				 | `4`            | `1.1754e-38` to `3.4028e+38`   |
| `float`    | `double`				 | `8`            | `2.2250e-308` to `1.7976e+308` |
| `float`    | `long double`			 | `10`           | `3.4E-4932` to `3.4E+4932`     |

## NORMINETTE
> At 42 School, it is expected that almost every project is written following the Norm, which is the coding standard of the school.

```
- No for, do...while, switch, case, goto, ternary operators, or variable-length arrays allowed;
- Each function must be a maximum of 25 lines, not counting the function's curly brackets;
- Each line must be at most 80 columns wide, with comments included;
- A function can take 4 named parameters maximum;
- No assigns and declarations in the same line (unless static);
- You can't declare more than 5 variables per function;
- ...
```

* [42 Norms](https://github.com/42School/norminette/blob/master/pdf/en.norm.pdf) - Information about 42 code norms. `PDF`
* [Norminette](https://github.com/42School/norminette) - Tool to respect the code norm, made by 42. `GitHub`
* [42 Header](https://github.com/42Paris/42header) - 42 header for Vim. `GitHub`

## LICENSE
<p>
This work is published under the terms of <a href="https://github.com/jotavare/minishell/blob/main/LICENSE">Unlicense</a>.
</p>
