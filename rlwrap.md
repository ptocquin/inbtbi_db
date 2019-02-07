# Autocompletion in SQLite3

Source: https://superuser.com/questions/499634/enable-autocomplete-in-an-sqlite3-interactive-shell/999132#999132

```bash
sudo apt install rlwrap

# Create a file containing SQL keywords for autocompletion
mkdir ~/.rlwrap
cat keywords > ~/.rlwrap/sqlite3_completions

# Add sqlite3 dot commands
echo ".help" | sqlite3 | grep -o "^\.[a-z]* " >> ~/.rlwrap/sqlite3_completions

# Add this line in .bashrc or .zshrc
alias sqlite="rlwrap -a -N -c -i -f ~/.rlwrap/sqlite3_completions sqlite3"
```
