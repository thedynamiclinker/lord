# Provider directories

`UNPATH` is a colon-separated list of local provider roots. Set `UNROOT` to map a working directory to the same relative directory in each provider. With `UNROOT` unset, each provider root corresponds to the current directory. `un path` can save a default `UNPATH`.

`o` links the first matching provider at `...`. `o PROVIDER` picks one by name or path. The open link takes precedence for subsequent commands. Use `un switch PROVIDER` to change it or `un close` to remove it. The link exposes ordinary, editable files and directories.

`dn` lists the entries available in the selected tree. Use `dn --cached` to list the local checkout offline. Load `source <(un completion)` in Bash for `dn <TAB>`. `dn NAME...` copies the named entries into the current directory. `up` sends updates to existing provider paths. `up NAME...` explicitly publishes files or whole directories. The default copies use rsync and exclude `.git` and `...`. A name beginning with a hyphen follows `--`.

`un clone REPO [BRANCH] DIR` makes an editable local checkout of a local or network Git remote. Add `DIR` to `UNPATH`. Git metadata lives under `${XDG_STATE_HOME:-$HOME/.local/state}/un/providers`, keyed by the checkout's absolute path. The checkout has no `.git` entry. Use separate checkouts to make multiple branches available and `un export REPO REF DIR` to make a plain directory from a particular revision.

Downloads, listings, and uploads fetch and fast-forward managed Git checkouts. An upload commits the selected paths and pushes to the upstream branch using the user's Git identity and credentials. Without names, only tracked paths in the current directory are eligible. Run `up` from inside the checkout to publish edits made there. Diverged branches and overlapping incoming edits require resolution before upload. A rejected push leaves the commit in the checkout.

`@` runs a command from `PATH` in the selected provider directory and defaults to `ls`. Use `@ cat FILE`, `@ cd [DIR]` with the Bash integration, or `@ chroot [COMMAND ...]` with the system's chroot privileges. Opening, shell commands and completion use the local checkout so they work offline.

Set `UN_SYNC` in the shell to an executable to replace listing and transfers. It receives the action (`list`, `dn` or `up`), the absolute working directory, the absolute provider directory, and all entry arguments. For `list`, print one entry per line. The backend controls its own storage, sync and history.
