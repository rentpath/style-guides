# Style Guides
A collection of style guides used at RentPath.

# Editor Setup

## Vim

- add to `.vimrc`
  - [rubocop plugin](https://github.com/ngmy/vim-rubocop)
  - `let g:vimrubocop_config = ~/source/style-guides/ruby/.rubocop.yml`
  - see `:RuboCop -h` for usage
- mappings
  - `nmap <Leader>r :echo @%\|RuboCop -a <CR>` " autofix current file
  - use `<Leader>ru` to lint the current file

