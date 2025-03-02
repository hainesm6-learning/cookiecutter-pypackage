# Tutorial

??? Note
    Did you find this article confusing? [Edit this file] and pull a request!

To start with, you will need [GitHub], [PyPI], [TestPyPI] and [Codecov] account. If
you don't have one, please follow the links to apply one before you get started on this
tutorial.

If you are new to Git and GitHub, you should probably spend a few minutes on
some tutorials at the top of the page at [GitHub Help].

## Step 1: Install Cookiecutter

Install cookiecutter:

``` bash
pip install cookiecutter
```

## Step 2: Generate Your Package

Now it's time to generate your Python package.

Run the following command and feed with answers, If you don’t know what to enter, stick with the defaults:

```bash
cookiecutter https://github.com/hainesm6-learning/cookiecutter-pypackage.git
```

Finally, a new folder will be created under current folder, the name is the answer you
provided to `project_slug`.

Go to this generated folder, the project layout should look like:

```
.
├── .editorconfig
├── .github
│   ├── ISSUE_TEMPLATE.md
│   └── workflows
│       ├── pull-request.yml
│       ├── push.yml
│       └── release.yml
├── .gitignore
├── .pre-commit-config.yaml
├── CONTRIBUTING.md
├── LICENSE
├── README.md
├── docs
│   ├── api.md
│   ├── contributing.md
│   ├── index.md
│   ├── installation.md
│   └── usage.md
├── mkdocs.yml
├── my_package
│   ├── __init__.py
│   ├── cli.py
│   └── my_package.py
├── pyproject.toml
├── setup.cfg
└── tests
    ├── __init__.py
    └── test_my_package.py

```

Here the project_slug is `my-package`, when you generate yours, it could be another name.

Also notice there's `pyproject.toml` in this folder. This is the main configuration file of our project.

## Step 3a: Install Poetry

In this step we will install Poetry if you are not using it, since the whole project is managed by it.
Poetry provides a [custom installer](https://python-poetry.org/docs/#installation) that will install
poetry isolated from the rest of your system by vendorizing its dependencies.
This is the recommended way of installing poetry.

## Step 3b: Instal tox

We also need to install tox outside of the poetry env in order to robustly run tests during development:

``` bash
pip install tox
```

## Step 4: Install Dev Requirements

You should still be in the folder named as `project_slug`, which contains the
 `pyproject.toml` file.

Install the new project's local development requirements with `poetry install`:

``` bash
poetry install -v
tox
```

Poetry will create its own virtualenv isolated from your system and install the dependencies in it.
We installed extra development dependencies by not specifying the `--no-dev` flag.

We also launch a smoke test here by running `tox`. This will run `tox` within a created virtual environment,
give you a test report and lint report. You should see no errors except some lint warnings.

You can also activate the virtual environment manually with `poetry shell`, this will create a new shell.

??? Tips

    if you found erros like the following during tox run:
    ```
    ERROR: InterpreterNotFound: python3.9
    ```
    don't panic, this is just because python3.x is not found on your machine. If you
    decide to support that version of Python in your package, please install it on your
    machine. Otherwise, remove it from tox.ini and pyproject.toml (search python3.x then
    remove it). It might also be that poetry isn't configured for the correct environment/s.
    If this is the case, you can remove and add environments with [poetry env](https://python-poetry.org/docs/managing-environments/)

## Step 5: Create a GitHub Repo

Go to your GitHub account and create a new repo named `my-package`, where
`my-package` matches the `project_slug` from your answers to running
cookiecutter.

Then go to repo > settings > secrets, click on 'New repository secret', add the following
 secrets:

- TEST_PYPI_API_TOKEN, see [How to apply TestPyPI token]
- PYPI_API_TOKEN, see [How to apply pypi token]

## Step 6: Set Up codecov integration

???+ Tips

    If you have already setup codecov integration and configured access for all your
    repositories, you can skip this step.

In your browser, visit [install codecov app], you'll be landed at this page:

![](http://images.jieyu.ai/images/202104/20210419175222.png)

Click on the green `install` button at top right, choose `all repositories` then click
on `install` button, following directions until all set.

If the repo you created is a private repo, you need to set the following additional secrets,
which is not required for public repos:

- CODECOV_TOKEN, see [Codecov GitHub Action - Usage](https://github.com/marketplace/actions/codecov?version=v1.5.2#usage)

## Step 7: Upload code to GitHub

Back to your develop environment, find the folder named after the `project_slug`.
Move into this folder, and then setup git to use your GitHub repo and upload the
code:

``` bash
cd my-package

git add .
git commit -m "Initial commit."
git branch -M main
git remote add origin git@github.com:myusername/my-package.git
git push -u origin main
```

Where `myusername` and `my-package` are adjusted for your username and
repo name.

You'll need a ssh key to push the repo. You can [Generate] a key or
[Add] an existing one. You also might want to checkout [keychain] to help manage ssh keys.

???+ Warning

    if you answered 'yes' to the question if install pre-commit hooks at last step,
    then you should find pre-commit be invoked when you run `git commit`, and some files
     may be modified by hooks. If so, please add these files and **commit again**.

### Check result

After pushing your code to GitHub, go to the GitHub web page, navigate to your repo, then
click on the actions link. You should find something like this:

![](http://images.jieyu.ai/images/202104/20210419170304.png)

There should be some workflows running. After they've finished, go to [TestPyPI], check if a
new artifact is published under the name `project_slug`.

You should also check the documentation which will be published and available at *https://{your_github_account}.github.io/{your_repo}*

## Step 8. Make official release

  After your done with phased development and have merged all changes into the main branch, create and publish a release (see [Managing GitHub releases]). This will trigger the first official release on [PyPI]!


[Edit this file]: https://github.com/waynerv/cookiecutter-pypackage/blob/master/docs/tutorial.md
[Codecov]: https://codecov.io/
[PYPI]: https://pypi.org
[GitHub]: https://github.com/
[TestPyPI]: https://test.pypi.org/
[GitHub Help]: https://help.github.com/
[Generate]: https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
[Add]: https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/
[How to apply testpypi token]: https://test.pypi.org/manage/account/
[How to apply pypi token]: https://pypi.org/manage/account/
[install codecov app]: https://github.com/apps/codecov
[keychain]: https://www.funtoo.org/Keychain
[Managing GitHub releases]: https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository