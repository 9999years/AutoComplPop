# AutoComplPop

This is a mirror of [AutoComplPop on vim.org][1]

## Introduction

Automatically opens the ins-completion pop-up menu whenever you're typing. It
won't prevent you from typing as normal.

## Installation

`git clone` to `$VIM/vimfiles/pack/autocomplpop/start/autocomplpop`, where it
should be loaded automatically on startup. Don’t have Vim 8 / `pack` support?
**Get Vim 8.**

## Commands

`:AcpEnable` enables auto-popup.

`:AcpDisable` disables auto-popup.

`:AcpLock` suspends auto-popup temporarily.

For the purpose of avoiding interruption to another script, it is
recommended to insert this command and `:AcpUnlock` than `:AcpDisable` and
`:AcpEnable`.

`:AcpUnlock` resumes auto-popup suspended by `:AcpLock`.

## Options

### `let g:acp_autoselectFirstCompletion = 0`

If `1`, the first value is always selected when the popup menu is shown
(so that `<CR>` or `<C-y>` insert the match). Otherwise, the first item is
*not* selected (like [YouCompleteMe][11]) and `<Tab>` is used to insert the
match.

### `let g:acp_enableAtStartup = 1`

If non-zero, auto-popup is enabled at startup.

### `let g:acp_mappingDriven = 0`

If non-zero, auto-popup is triggered by key mappings instead of
[CursorMovedI][5] event. This is useful to avoid auto-popup by moving
cursor in Insert mode.

### `let g:acp_ignorecaseOption = 1`

Value set to [`ignorecase`][6] temporarily when auto-popup.

### `let g:acp_completeOption = '.,w,b,k'`

Value set to [`complete`][7] temporarily when auto-popup.

### `let g:acp_completeoptPreview = 0`

If non-zero, `preview` is added to ['completeopt'][8] when auto-popup.

### `let g:acp_behaviorUserDefinedFunction = ''`

`g:acp_behavior-completefunc` for user-defined completion. If empty,
this completion will be never attempted.

### `let g:acp_behaviorUserDefinedMeets = ''`

`g:acp_behavior-meets` for user-defined completion. If empty, this
completion will be never attempted.

### `let g:acp_behaviorSnipmateLength = -1`

Pattern before the cursor, which are needed to attempt
snipMate-trigger completion.

### `let g:acp_behaviorKeywordCommand = "\<C-n>"`

Command for keyword completion. This option is usually set `\<C-n>` or
`\<C-p>`.

### `let g:acp_behaviorKeywordLength = 2`

Length of keyword characters before the cursor, which are needed to
attempt keyword completion. If negative value, this completion will be
never attempted.

### `let g:acp_behaviorKeywordIgnores = []`

List of string. If a word before the cursor matches to the front part
of one of them, keyword completion won’t be attempted.

E.g., when there are too many keywords beginning with “get” for the
completion and auto-popup by entering “g”, “ge”, or “get” causes
response degradation, set [“get”] to this option and avoid it.

### `let g:acp_behaviorFileLength = 0`

Length of filename characters before the cursor, which are needed to
attempt filename completion. If negative value, this completion will
be never attempted.

### `let g:acp_behaviorRubyOmniMethodLength = 0`

Length of keyword characters before the cursor, which are needed to
attempt ruby omni-completion for methods. If negative value, this
completion will be never attempted.

### `let g:acp_behaviorRubyOmniSymbolLength = 1`

Length of keyword characters before the cursor, which are needed to
attempt ruby omni-completion for symbols. If negative value, this
completion will be never attempted.

### `let g:acp_behaviorPythonOmniLength = 0`

Length of keyword characters before the cursor, which are needed to
attempt python omni-completion. If negative value, this completion
will be never attempted.

### `let g:acp_behaviorPerlOmniLength = -1`

Length of keyword characters before the cursor, which are needed to
attempt perl omni-completion. If negative value, this completion will
be never attempted.

See also: `acp-perl-omni`

### `let g:acp_behaviorXmlOmniLength = 0`

Length of keyword characters before the cursor, which are needed to
attempt XML omni-completion. If negative value, this completion will
be never attempted.

### `let g:acp_behaviorHtmlOmniLength = 0`

Length of keyword characters before the cursor, which are needed to
attempt HTML omni-completion. If negative value, this completion will
be never attempted.

### `let g:acp_behaviorCssOmniPropertyLength = 1`

Length of keyword characters before the cursor, which are needed to
attempt CSS omni-completion for properties. If negative value, this
completion will be never attempted.

### `let g:acp_behaviorCssOmniValueLength = 0`

Length of keyword characters before the cursor, which are needed to
attempt CSS omni-completion for values. If negative value, this
completion will be never attempted.

### `let g:acp_behavior = {}`

This option is for advanced users. This setting overrides other
behavior options. This is a [`Dictionary`][10] where each key corresponds to a
filetype. `'*'` is default. Each value is a list. These are attempted in
sequence until completion item is found. Each element is a
[`Dictionary`][9] which has following items:

#### `"command"`

Command to be fed to open popup menu for completions.

#### `completefunc`

`completefunc` will be set to this user-provided function during the
completion. Only makes sense when `command` is `<C-x><C-u>`.

#### `meets`

Name of the function which dicides whether or not to attempt this
completion. It will be attempted if this function returns non-zero.
This function takes a text before the cursor.

#### `onPopupClose`

Name of the function which is called when popup menu for this
completion is closed. Following completions will be suppressed if
this function returns zero.

#### `repeat`

If non-zero, the last completion is automatically repeated.


### E219 Missing {.

This error is often caused by Vim interpreting a TeX command that ends
a line as a filename and attempting to complete it. Something like {}
is invalid in a filename, so it errors. Fix by inserting

    let g:acp_behaviorFileLength = -1

In your [vimrc][12] or a specific [ftplugin][13] (like
`$VIM/vimfiles/ftplugin/tex.vim`).

See also `g:acp_behaviorFileLength`.


## Further Help

See `:h acp` or [the regular text file in this repository][2]

## Compatibility

[snipMate][3]’s trigger completion enables you to complete a snippet trigger
provided by snipMate plugin and expand it.

To enable auto-popup for this completion, add following function to
snipMate’s `plugin/snipMate.vim`:

    fun! GetSnipsInCurrentScope()
      let snips = {}
      for scope in [bufnr('%')] + split(&ft, '\.') + ['_']
        call extend(snips, get(s:snippets, scope, {}), 'keep')
        call extend(snips, get(s:multi_snips, scope, {}), 'keep')
      endfor
      return snips
    endf

And set `g:acp_behaviorSnipmateLength` to `1`.

Please note that the popup is then restricted to situations where the cursor
follows an all-caps word.

AutoComplPop supports [`perl-completion.vim`][4] — To enable popups, set
`g:acp_behaviorPerlOmniLength` option to `0` or more.

[1]: http://www.vim.org/scripts/script.php?script_id=1879
[2]: https://github.com/vim-scripts/AutoComplPop/blob/master/doc/acp.txt
[3]: https://github.com/garbas/vim-snipmate
[4]: https://github.com/vim-scripts/perlomni.vim
[5]: http://vimdoc.sourceforge.net/htmldoc/autocmd.html#CursorMovedI
[6]: http://vimdoc.sourceforge.net/htmldoc/options.html#'ignorecase'
[7]: http://vimdoc.sourceforge.net/htmldoc/options.html#'complete'
[8]: http://vimdoc.sourceforge.net/htmldoc/options.html#'completeopt'
[9]: http://vimdoc.sourceforge.net/htmldoc/eval.html#Dictionary
[10]: http://vimdoc.sourceforge.net/htmldoc/eval.html#Dictionary
[11]: https://github.com/Valloric/YouCompleteMe
[12]: http://vimdoc.sourceforge.net/htmldoc/starting.html#vimrc
[13]: http://vimdoc.sourceforge.net/htmldoc/usr_41.html#ftplugin
