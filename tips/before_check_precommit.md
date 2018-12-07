# Automatic Code Quality Checks

## pre-commit
 - can change commit message or prevent commit

## Hook and install script

There’s one problem. Files stored inside a .git directory are not kept in the repository.

We can deal with it by creating our hook in scripts and creating symlink from .git/hooks directory. Also, this will keep our hook always in sync.

- Let’s create scripts/pre-commit.bash hook.

```
#!/usr/bin/env bash

echo "Running pre-commit hook"
./scripts/run-tests.bash

# $? stores exit value of the last command
if [ $? -ne 0 ]; then
 echo "Tests must pass before commit!"
 exit 1
fi
```

- And final step is to create `scripts/install-hooks.bash` script

```
#!/usr/bin/env bash

GIT_DIR=$(git rev-parse --git-dir)

echo "Installing hooks..."
# this command creates symlink to our pre-commit script
ln -s ../../scripts/pre-commit.bash $GIT_DIR/hooks/pre-commit
echo "Done!
```

- Let’s make all new scripts executable

```
chmod +x scripts/run-tests.bash scripts/pre-commit.bash scripts/install-hooks.bash
```

- Feel free to install our hook

```
./scripts/install-hooks.bash
```

- And testing

+ Example content of a scripts/run-tests.bash file:

```
#!/usr/bin/env bash

# if any command inside script returns error, exit and return that error 
set -e

# thanks to it we can just enter `./scripts/run-tests.bash`
cd "${0%/*}/.."

echo "Running tests"
echo "............................" 
echo "Failed!" && exit 1
```

```
git add .
git commit -m "test"
>> Running pre-commit hook
>> Running tests
>> ............................
>> Failed!
>> Tests must pass before commit!
```

## Cheating

If we really have to skip tests we can use --no-verify flag like this:

```
# pre-commit hook is skipped
git commit --no-verify -m "test"
```
