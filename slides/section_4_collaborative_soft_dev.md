---
jupyter:
  celltoolbar: Slideshow
  jupytext:
    notebook_metadata_filter: -kernelspec,-jupytext.text_representation.jupytext_version,rise,celltoolbar
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
  rise:
    theme: solarized
---

<!-- #region slideshow={"slide_type": "slide"} -->
# Section 4: Collaborative Software Development for Reuse

</br>
</br>
<center><img src="../fig/section4-overview.png" width="70%"></center>
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Up until this point, the course has been primarily focussed on
  technical practices, tools, and infrastructure.
- In particular, we've been looking at things from the perspective of
  a lone developer.
- In this section of the course we're going to be looking at how we develop
  software collaboratively.

- We'll cover some collaborative practices:
  - Code review,
  - A bit on documentation, and
  - Release packaging - how we can make life easy for would-be collaborators
    by making our software easy to install.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Quick Check: Who Has All the Branches!

Check if you have a branch named `remote/origin/feature-std-dev` or `feature-std-dev` after running:

```bash
git branch --all
```

If not, please run these commands:

```bash
git remote add upstream git@github.com:ukaea-rse-training/python-intermediate-inflammation.git
git fetch upstream
git checkout upstream/feature-std-dev
git switch --create feature-std-dev
git push --set-upstream origin feature-std-dev
```
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- Before we start, if you run `git branch --all`,
  do you see these branches?
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Developing Software in a Team: Code Review

Two main ways to collaborate with git:

1. Fork and Pull Model
2. Shared Repository Model

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

### Fork and Pull Model

- Anyone can create a **fork** of an existing repository.
- This gives a developer a copy of the repository
  on which they can work independently.
- The changes made on the **fork** can then be reviewed by
  the **upstream** maintainer, before being merged into the
  **upstream** repository.
- This is a popular model with open source projects,
  as it reduces the start-up costs for new contributors
  and allows them to work independently,
  without coordinating with the project maintainers.
- You'll commonly do this if you're an external collaborator
  on a project, rather than a core developer.

### Shared Repository Model

- Collaborators create branches in the main repository.
- Requests are opened to merge changes on
  collaborators' branches into the main branch.
- Typically, no one is allowed to push to the main branch directly.
  - All changes are made via pull (or merge) requests,
    so the team have a change to _review_ changes.

TODO make a nice mermaid diagram for this

- In the absence of a nice diagram, draw something on the whiteboard for the above
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Code Review

> Code review (n.): a software quality assurance practice where one or several people from the team, different from the code’s author, check the software by viewing parts of its source code, making comments, and rejecting or approving those changes
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- So far you have been merging code into the main branches of your
  repositories yourselves.
- This is not how collaborative programming is typically done.
- Instead, there is a gate check before anything gets merged into the main
  branch of a repo.

- Code review has lots of benefits:
  - You're sharing knowledge of the codebase between the team.
    - Reduce the bus factor!
    - Not only are reviewers aware of any new code going into the codebase,
      they can also make others aware of pre-existing functionality that
      the reviewee may not know of.
  - You're sharing general software engineering knowledge too.
  - Having to explain your code to someone can also clarify your understanding.
  - Catching problems early saves time!
    - Having to revert changes after-the-fact is far more time consuming.
  - Knowing your code is to be reviewed keeps you honest.
    - Your code has to be up to standard and understandable to be accepted.
    - Cutting corners isn't an option.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "fragment"} -->
Lots of benefits:

1. 👥 Knowledge sharing: improve redundancy in the team.
2. 🧠 Explanation improves understanding and rationale. Better decisions are made.
3. ❌ Reduce errors in code. Between 60 and 90% of errors can be caught by rigourous code review (Fagan, 1979).
   - Errors caught earlier are 10 to 100 times less expensive or time-consuming to fix.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Types of Code Review

1. Over-the-shoulder review
2. Pair programming
3. Formal code inspection
4. **Asynchronous, tool-assisted review ⬅️**
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- There are a variety of different code review techniques

1. Over-the-shoulder code review
  - Chat through the changes in person at a computer.

2. Pair programming
  - Two developers work on the same code at the same time.

3. Formal code inspection
  - have up to 6 participants go through a formal process to inspect code.
  - dreamed up by IBM in the 70s.
  - involves several stages, including presenting changes to the group.

4. Tool assisted code review
  - Use tools such as GitHub to review code asynchronously and give feedback.

- The most commonly used, in my experience, is tool assisted.
- We'll be using GitHub's Pull Request features to review our code.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Code Review Exercise Steps

<center><img src="https://mermaid.ink/img/pako:eNptkstOwzAQRX_FmnWo0pfTZFEpgm03qRASysbUU2opsYM9Li1V_h3nAaVAVo7PnXttz1xgZyRCBg7fPOodPijxakVdaha-RlhSO9UITSxnwrHc08HYv7DoYIFHhe844vxuvc4z9mQVIXOmRtYlXVmRsUIoh0ywxlcVs90BHA2CYhDkUoaqukZNjpEJUttnXEUhYetfakW_WGVMwx41qYqJprHmiHIAP-KDu0XnmLGh1DVGyy5jcPmOvVaNcfeVsGp_HqtMdcRfWtTy5nz5kP_PLccn2qB9vcUQQY22FkqGzlw6cQl0wBpLyMJS4l74ikqIBqS0oo5cwvqrj_1_Cd7hRpyelKRD2NmLymHL2lK3IUJ4Mtuz3kFG1mMEvpGCvgZgFEcQ-gvZBU6QzeN0ksScp8vlkvNFzCM4Qzad87Cbrng6Xy0SvuSzNoIPY4JDPFnFs8VqmvKEz-Jknix6u-cejvYoFRm7GYawn8X2E-o50Ms?type=png"></center>

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
TODO the source of the png is online from mermaid.ink editor. We should figure a way to incorporate mermaid into these slides directly.

### Reviewing a Pull Request

- Once a pull request has been opened,
  it is over to the reviewer to submit a review.
- Once a review has been submitted, the pull request author
  should make relevant changes/respond to comments.
- The reviewer then reviews that latest changes.
- Once the reviewer is happy, they will "Approve" the PR.
- The author or reviewer can then merge the change.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Exercise: Raising a pull request for your fictional colleague

Go through the steps described under heading. Stop when you reach **Reviewing a pull request**
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- We're now going to open a pull request in GitHub which we'll later review.
  - Follow the steps in the exercise in the notes.

- Should be pretty quick, 5 minutes max.
- Status check then move on.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Exercise: review some code

Pair up with someone else in your group and exchange repository links. You will be taking on the role of _Reviewer_ on your partner's repository. Before leaving review comments, read the content under the heading **Reviewing a pull request**. Try to make a comment from each of the main areas identified.

**Don't submit your review just yet!!!**
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Now that everyone's opened a PR,
  we're going to pair up and review each others'.
  - If anyone's not in a pair, we can round-robin in a three.
- We're only reviewing the code at the moment,
  we'll review the tests afterwards.

- Have a read through of the guidance in the notes
  and try to follow the recommended practices.

- Positive comments are also worth thinking about.

- Don't submit the review yet, just add your comments.

- 10-15 minutes
- Status check then move on.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Exercise: review the code for suitable tests

Add a list of expected tests to the comment box after clicking `Finish your review` near the top right of the `Files changed` tab. Use the content under **Making sure code is valid** to come up with these tests, and think back to the requirement SR1.1.1.

When done, select `Request changes` from the list of toggles, then `Submit review`.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- To finish the review, we're going to think about tests.
- Based on the specification, write a list of the tests you'd expect
  to see.
- As you go through the code, add to this list any more tests you can
  think of.
- This is all in the notes, so just follow along.

- 10-15 minutes
- Status check then move on.

- When you're done, select 'Request Changes` and 'Submit Review'.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Exercise: responding and addressing comments

Respond to the _Reviewers_ comments on the PR in _your_ repository. Use the information in **Responding to review comments** to guide your responses. And remember that you can talk to your _Reviewer_ for clarification, just make sure you record that in a comment on the PR.

Do not implement changes that will take more than 5 minutes. Instead, raise them as an issue on your repo for future work, and link to that issue in a comment on the PR.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Next, we're going to be responding to the PR comments we've just
  received.
- Update any code that needs updating in your branch and push it.
- Respond to any questions, e.g., if the reviewer is seeking some
  clarification on something.
- Or respond to any comments that you feel need further discussion.

- 10-15 minutes
  - Don't worry too much about implementing changes.
    If you think the change will take more than a few minutes,
    open an issue to resolve the comment, and reply to the comment
    with a link to the issue.
- Status check then move on.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Making code easy to review

- 🤏 Keep the changes small.
- 1️⃣ Keep each commit as one logical change.
- 🪟 Provide a clear description of the change.
- 🕵️ Review your code yourself, before requesting a review.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- The most important thing really is the size of the PR.
  - If your PR is very big, reviewing it will be hard.
  - When reviewing is difficult, things fall through the cracks.

- Try to keep commits logically distinct.
  - The commit history is an ordered list of the changes you've made.
  - It can be useful for a review to see the process you've taken.

- Write a good PR description.
  - This description serves a couple of purposes:
    - states what changes have been made/what new thing has been added.
    - guides the reviewer through _how_ to review the change.
      It may make sense to look at file A before B for example.
      Also, if they're doing some manual testing too,
      describe how you expect the new functionality to behave.

- It's also helpful to review your code yourself
  before requesting a review from a collaborator.
  - The diff shown on GitHub before you open the PR is very useful.
  - I often catch things this way.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Empathy in review comments

- Identify positives in code as and when you find them.
- Remember different doesn't mean better.
- Ask questions to understand why something has been done a certain way rather than assuming you
  know a better way.
- If a conversation is taking place on a review and hasn't been resolved by a
  single back-and-forth exchange, then schedule a conversation to discuss instead
  (recording the results of the discussion in the PR).
- Code review is chance to learn from one another!
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Code review is an opportunity for sharing knowledge and encouraging
  each other to become better developers.
  - Try to have this mindset when reviewing others' code.
- Identify positives
  - Is there an interesting approach to a problem?
  - Is there a language feature you didn't know about?

- Also remember that the person who has written the code has, most likely,
  spent longer on this problem than you have.
  - Ask questions to understand why something has been implemented a certain
    way.
  - Remember there is usually more than one way to solve a problem.
  - Just because it's not how you would do it, doesn't mean it's bad.

- Phrase your suggestions as comments, not commands!
  - Consider using collective terms like 'we', rather than 'you'.
    - 'Might we consider X, instead of Y?'

- Consider taking complex conversations offline,
  and writing a summary in the comments.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Exercise: Code Review in Your Own Working Environment

Follow the instructions under this exercise heading. Read the content above the exercise to figure out what is involved in a code review process for a team. After about 5 minutes, have a small conversation in your group about what your code review process would look like.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Have a read through of the last exercise in this episode.
- 5 minutes, then we'll have a quick discussion on your thoughts.

- Benefits:
  - Notifying others of changes being made.
  - Sharing knowledge beyond just the codebase.
  - Forcing us to break our work down into reviewable chunks.

- Cons:
  - If not done correctly can lead to awkward team dynamics.
  - Can be difficult to review if other team members are not
    making "reviewable" changes.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Preparing Software for Reuse and Release
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- In this episode of the course, we're going to look at software **reuse**.
- For people to collaborate on your software,
  they need to be able to install it and understand enough to contribute.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
- 🔁 We want our code to be somewhere on the "reusablility" spectrum
- 📝 Documentation is an important part of our code being reusable
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- The 5 Rs of reusablility:
  - Re-runnable
  - Repeatable
  - Reproducible
  - Reusable
  - Replicable

- We want our code to be somewhere on the "reusablility" spectrum.
  - How reusable your software needs to be depends on your use case.
  - For research software, it must at least be "reproducible"
    - i.e., people can reproduce results you present in your paper.
  - Open source libraries, e.g., Numpy,
    probably need to be closer to "Resuable"
    - As they aim to be easy to use and understand,
      and they want open source contributors.

- Documentation is an important part of our code being reusable.
  - Even if you write incredibly expressive code,
    it will not be enough for newcomers to start using and modifying your
    codebase.
  - How do we install it?
  - What are the coding standards?
  - What development tools will I need?
  - What is the background information needed to understand the code
    (e.g, some maths).
  - We need to answer all of these questions and more if we want our code to be approachable and reusable

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Breakout: Start from the Top

Start from the top of this episode page (4.2) and go to the end.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Read through to the end of this section of the notes.
- Sorry that this is a bit dry.
- Don't worry about the exercises, or adding any of this to your project.
  - It's just things to be aware of, you can always come back to these notes
    or consult Google.

_Write some notes to discuss at the end of this section!_

- A preface note: if you have been using codimd or hackmd for the shared document, then learners will have already been exposed to Markdown, so this section won't contain much new for them

- Post episode comments
  - A README is a great place to start your documentation, but at some point it will outgrow that, and you will need a bigger documentation system. The most popular in Python is Sphinx, which can be used with Markdown or another markup language called ReStructuredText (`.rst` files)
  - For writing documentation, this is another great link that can be added to the shared document: https://documentation.divio.com/
  - For licensing software, make some notes in the shared document about the policy of your institution
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## ☕ Break Time ☕
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## Packaging Code for Release and Distribution
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
### Why Package Our Software?

- ⏬ It reduces "the complexity of fetching, installing, and integrating it [our code] for the end-users"
- 📦 Packaging combines the relevant source files and necessary metadata to achieve the above
- 😕 Confusing term, _package_
  - module _package_ : a directory containing Python files and an `__init__.py` file
  - distributable _package_ : a way of structuring and bundling a Python project for easier distribution and installation
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- Why do we want to package our software?
- We want to minimise the complexity of using our software for users.
  - First impressions matter.
  - If we want people to use our software,
    making it easy to install is important.

- Packaging things in standard ways simplifies interactions with the
  wider ecosystem.

- There is some mixing of terminology here,
  we have 2 definitions of a package:
  - A directory containing an init.py - a "module" package.
  - A way of bundling a project for distributing - a "distributable package".
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Packaging Our Software with Poetry

- 📌 Pinning dependencies in a `requirements.txt` has some limitations
- 📜 Poetry is a tool that helps overcome some of these deficiencies
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- So far, we've been pinning our dependencies in a `requirements.txt` file.
- This has some serious limitations.
- It does not account for application vs. library dependencies.
  - Application dependencies could and should be pinned to specific versions.
  - Libraries need to have looser version constraints.
- It is error prone:
  - We may pip install a library and our code works,
    but we forgot to add it to our requirements!
- No dependency resolution
  - Dependencies are installed in order,
    we do not solve the 'constraint' problem to ensure we do not have
    dependency conflicts.
  - We'd only find out about these conflicts at runtime,
    and suddenly we can't install a valid environment for our code.

- Distributing Python packages requires more metadata than can be
  specified in a `requirements.txt` file.

- To handle our dependencies better and streamline our packaging,
  we're going to use poetry.
- We add dependencies through poetry,
  and it automatically records them.
- It can separate application and library dependencies
  - I.e., "locked" dependencies and constrained dependencies.
- Poetry performs dependency resolution,
  so it won't let us add a dependency if there are conflicts.
- It performs much of the work needed for packaging!
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Installing Poetry

#### ⚠️ Warning ⚠️

> The documentation for Poetry explicitly discourages installing Poetry into your current virtual environment.
> Therefore, please use the installation instructions from their website.

Since we are all on Linux, it should roughly be:

```bash
curl -sSL https://install.python-poetry.org | python3 -
ls $HOME/.local/bin  # make sure poetry executable is listed there
which poetry  # if no output, then poetry not in your path
poetry --version  # check we have access to the poetry executable
```
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- A deviation from the course material here.
- Do **not** install poetry inside your virtual environment.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Setting up Our Poetry Config

- The current way of sharing our package is:

  ```bash
  git clone <our_repo>
  python -m venv venv
  . venv/bin/activate
  pip install -r requirements.txt
  python inflammation-analysis.py
  ...
  ```

  - and then there are a bunch of hoops to jump through to make sure import statements work when testing
- What if someone wants to just `pip install` our package?
- Poetry helps us with this
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- The current way of sharing our package is... clunky!!!
  - and then there are a bunch of hoops to jump through to make sure import statements work when testing
- What if someone wants to just `pip install` our package?
- Poetry helps us with this
  - we need to define some metadata for our project so that poetry can properly install it in a Python environment
  - this is done in a `pyproject.toml` file that `poetry` can help us generate pretty quickly

- I'll demo how to initialise our poetry project.
  - Note that we call the package `inflammation` because then it will
    automatically find the package in the repo's directory structure.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Project Dependencies

We will look at two types of dependencies:

1. Runtime dependencies
2. Development dependencies
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "fragment"} -->
Runtime dependencies can be further subdivided:

1. _Pinned_ runtime dependencies when our package is used as a standalone application
2. _Looser_ runtime dependencies when our package is used as a library
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->
- We'll look at a couple of types of dependency:
  - Runtime
  - Development

- As mentioned before, we'll also make a distinction between
  - locked (or pinned) for applications.
  - constrained (or looser) for libraries.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Project Dependencies

Commit your initial `pyproject.toml` into git. Then, run the commands:

```bash
poetry add matplotlib numpy
poetry add --group dev pylint
poetry install
```

Inspect how `pyproject.toml` has changed. Look at what has gone into `poetry.lock`.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- Run these commands and have a quick look at the updates poetry has
  made to your pyproject and poetry.lock files.

- _Show pyproject and lock files._

- Don't check `poetry.lock` into version control if you are developing a
  library.

- The `poetry install` command installs our package into the virtual
  environment poetry creates for us.

<!-- #endregion -->

<!-- #region slideshow={"slide_type": "subslide"} -->
### Packaging Our Code

Now that we have a `pyproject.toml` file, building a distributable package is as easy as:

```bash
poetry build
```

🤯🤯🤯
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "notes"} -->

- _Run poetry build_.

- _Look at the two files in the `dist/` folder._
- The `.whl` file is a Python wheel
  - We can send this to someone and they can `pip intstall` it!

- This is the file you will typically upload to PyPI, or attach to a GitHub
  release.

- The file is just a zip that contains our Python code and metadata.

  `unzip x.whl -d x`

- Note that this only contains our package,
  not our tests or data etc.

- Demo installing the package into a new virtual environment.

- There are tools other than poetry you can use to handle dependencies and
  packaging.
  - Check out hatch: https://github.com/pypa/hatch
  - Which has been gaining some momentum recently.
<!-- #endregion -->

<!-- #region slideshow={"slide_type": "slide"} -->
## 🕓 End of Section 4 🕓

<!-- #endregion -->
