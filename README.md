# cogfetch
Yet, yet another fetcher written in Bash.

# Dependencies
You are fine, as long you have these in your system:
- bash
- echo, cat
- seq, awk, sed, wc
- find, which
- df, free, uname, uptime, hostname, whoami, lsb_release

# Installation
Simply place "cogfetch" somewhere within reach of your environment variable "PATH".
Example:
```
$ mv ./cogfetch /usr/local/bin/
```

# Configuration
Configuration files are just Bash scripts that gets sourced. They are supposed to define variables and "gathers".
### System-wide
Create file "cogfetch" under "/etc" directory:
```
$ echo > /etc/cogfetch
```
### Per-user
Create file ".cogfetch" under your user directory:
```
$ echo > ~/.cogfetch
```
### Variables you can define:
- **DEFAULT_GATHERS** - Gathers that will be used by default when "cogfetch" was ran without arguments. Default: 'host uptime os kernel pkgs mounts mem bat colors';
- **PKG_MANAGERS** - Array of package managers where order is:
- 1. Label that will be used in the key;
  2. Command from the package manager;
  3. Options for the package manager to list packages.
- **ART** - Optional array of strings that resembles art. Width is measured automatically (Can't guarantee reliability);
- **DELIMITER_CHAR** - Optional string that will be printed between the key and value. Default: ':';
- **SPACE_FROM_START** - n-spaces from the left wall before printing the line. Default: 1;
- **SPACE_FROM_ART** - n-spaces from the art before printing the key. If variable ART is not defined then ignored. Default: 1;
- **SPACE_FROM_KEY** - n-spaces from the key before printing delimiter string;
- **SPACE_FROM_DELIMITER** - n-spaces from the delimiter string before printing value. If variable DELIMITER_CHAR is not defined then ignored. Default: 1;
- **COLOR_KEY** - ANSI color that will be used when printing key. Default: '1;32';
- **COLOR_DELIMITER** - ANSI color that will be used when printing delimiter. Default '1;35';
- **COLOR_VALUE** - ANSI color that will be used when printing value. Default: '0'.
### "Gathers" 
Gathers are functions that can be used by "cogfetch" to gather and output specific information. Every gather supposed to start with "get_" and use function "log" to output result.
Example:
```
# First define gather somewhere in your configuration file
get_example()
{
   log 'Keyname' "Sample Text"
}

# Then run cogfetch with name of your gather as argument
$ cogfetch sampletext

# Or add it to your DEFAULT_GATHERS
DEFAULT_GATHERS+=' example'
```
### Example configuration
```
DEFAULT_GATHERS='host uptime os kernel pkgs mounts mem bat colonthree colors'
PKG_MANAGERS=('XBPS' 'xbps-query' '-l')
ART=()
DELIMITER_CHAR=':'
SPACE_FROM_START=0
SPACE_FROM_ART=2
SPACE_FROM_KEY=2
SPACE_FROM_DELIMITER=2
COLOR_KEY='1;32'
COLOR_DELIMITER='0'
COLOR_VALUE='0'

get_colonthree()
{
   log 'Nya' ':3'
}
```

# Usage
Simply run "cogfetch" to show information gathered by gathers you provided as arguments, or by defining variable "DEFAULT_GATHERS":
### Without arguments
```
$ cogfetch
```
### Using arguments 
```
$ cogfetch host uptime os kernel
```
However, this way you will overwrite variable "DEFAULT_GATHERS". If this behavior is unwanted then add "+" to append already existing variable:
```
$ cogfetch kernel uptime +
```
*(Order doesn't matter)*
```
$ cogfetch + kernel uptime
```
