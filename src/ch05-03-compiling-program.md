# Compiling your program

Thanks to redox' cookbook, building programs is a snap. In this example, we will be building the helloworld program that `cargo new` automatically generates.

## Step One: Setting up your program

To begin, go to your projects directory, here assumed to be `~/Projects/`. Open the terminal, and run `cargo new helloworld --bin`. For reasons that will become clear later,
you must make your program compile from a git repository, so run `cd helloworld;git init` to make helloworld a git repo, and `git status` to see which files you need to add to the repo. You should see something like this

```bash
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	Cargo.toml
	src/

nothing added to commit but untracked files present (use "git add" to track)
```

Add all the files by running `git add .gitignore Cargo.toml src/main.rs` and commit by running `git commit -m "Added files to helloworld." .gitignore Cargo.toml src/main.rs`

There is only one more thing that must be done to set up your program. Go to the `Cargo.toml` of your project and add 

```toml
[[bin]]
name = "helloworld"
path = "src/main.rs"
```
to the bottom. Now the program is sufficiently set up, and it is ready to be added to your redox build.

## Step Two: Adding your program to your redox build

To be able to access your program from redox, it must be added to both the build process and the filesystem. Adding your program to the filesystem is easy: go to your `redox/filesystem.toml` file, and look under the `[packages]` table. It should look like this:

```toml
[packages]
#acid = {}
#binutils = {}
contain = {}
...
userutils = {}
```

Under `userutils = {}` add a line for your own helloworld program:

```toml
userutils = {}
helloworld = {}
```

Now when building the filesystem, redox will look for a `helloworld` binary.

With the file system in order, you can now add your program to the build process by adding a recipie. Spoilers: this is the easy part.

Under `redox/cookbook/recipes/`, make a new directory called helloworld. In helloworld, create a file called `recipe.sh`.
cd redox/cookbook/recipes/
mkdir helloworld
cd helloworld
touch recipe.sh

Remember the git repository `~/Projects/helloworld/`? Its about to be relevant. In the file `recipe.sh`, write

```bash
GIT=~/Projects/helloworld/
```

With that, helloworld will now be built with and accessible from redox.

## Step Three: Running your program

Go up to your `redox/` directory and run `make all`. Once it finishes running, run `make qemu`, log in to redox, open the terminal, and run `helloworld.` It should print

```shell
Hello, world!
```

## Step Four: Iterate Build and Run Your Project

After modifying your project's helloworld/src/main.rs, you will want to build and run helloworld without having to rebuild the entire redox os again.  The best way to do is follows:

```bash
cd redox/
make env
cd redox/cookbook/
./repo.sh helloworld
make mount
```

Copy the helloworld binary to redox mounted partition and chmod +x helloworld
```bash
make qemu
```