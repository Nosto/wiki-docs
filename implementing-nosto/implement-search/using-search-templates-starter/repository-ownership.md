# Repository Ownership

Each Search Templates Starter project lives in its own Git repository. To keep development clean and accountable, that repository should be owned by whoever is actively developing or maintaining the template.

## Who should own the repository?

The principle is simple: **The primary developer of the template owns the template and its repository.**

* If **external (non-Nosto) developers** or their agency are building and maintaining the template, they should own the repository.
* If **Nosto** worked on the template previously and development is being handed over to the merchant or their agency, ownership should be **transferred** to the receiving party.

Keeping ownership aligned with active development means the people responsible for the code have full control over it.

> **Note:** To receive Nosto support when something goes wrong and you can't solve it, you should provide us access to the codebase one way or another.

## Transferring repository ownership

When ownership changes hands, the entire repository state needs to be transferred, including the commit history. There are multiple ways to do it - for example, GitHub ownership transfer tool, or a Git **bundle** - a single file that packages the whole repository and can be transferred over easily.

### GitHub ownership transfer tool

GitHub offers a [repository transfer tool](https://docs.github.com/en/repositories/creating-and-managing-repositories/transferring-a-repository) that can be used to transfer ownership of a repository. If you have full access to the repo, this is arguably the simplest way to transfer ownership.

### Exporting the repo manually

If the ownership tool is not applicable for one reason or another, you can go the manual route.

#### Step 1 - Clone the repository

Even if you already have the repository locally, re-clone it elsewhere as a mirror. This is different from a regular clone, as it captures the entire repository state.

```sh
git clone --mirror {repository-link}
```

This produces what is known as a **bare clone** in a folder named `repository-name.git`. It is essentially the raw Git structure rather than the code as you usually see it.

#### Step 2 - Bundle it into a single file

```sh
git -C repository-name.git bundle create repository-name.bundle --all
```

This creates a file named `repository-name.bundle` inside the directory. That single file is what you transfer over.

> **Note:** The bundle can be large for older codebases, but for Starter-based repositories it should typically be in the tens of megabytes.

#### Step 3 - Sanity check and send it

Before sending, verify the bundle was packed correctly by trying to clone from it (using the same command the receiving side will run - see [Importing the repo](#importing-the-repo) below). Once you've confirmed it clones cleanly, send the bundle file to the new owner through a secure channel.

### Importing the repo

#### Step 1 - Create your own Git repository

Create a new repository on GitHub, your preferred Git cloud provider or other hosting option in the usual way.

#### Step 2 - Clone the repository from the bundle

After receiving the bundle, clone the repo from the file as you would from a regular remote repository.

```sh
git clone repository-name.bundle {folder-to-clone-into}
```

#### Step 3 - Point the remote at your repository and push

Update the cloned repository's remote to point at your new GitHub repository, then push.

```sh
git remote set-url origin {your-repo-link}
git push --all
git push --tags
```

Now the repository has a new owner!
