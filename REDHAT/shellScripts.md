# Cutmoize Vim 
```sh
~/.vimrc
```
```sh
abbr _bash #!/bin/bash<CR>
set bg=dark 
# Example file 
vim /usr/share/vim/vim82/vimrc_example.vim
```
# Variavles 
## BASH variables 
```sh
user_name=bob # var with no spaces 

echo "$user_name" # Reading var 

echo "$(user_name)s" # Delimating variables 
```

## built-in variables 
```sh
$1,$2,,,${10} 
$0 # represents the script name 
$# # the number of parameters 
$* #list of the parameters
```
### default values
```sh
user_password=${2:-Password1}
```
## Reading user input 
```sh
read -p "Enter a username: " username 
read -sp "password: " password
read -e 
```
## using conditional statements 
```sh
if [ $# -eq 2 ] ; then  # note the space between brackets 
    # code 
fi 
```
# Functions and loops
