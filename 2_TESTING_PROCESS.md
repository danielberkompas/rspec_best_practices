# Testing Process
Before getting into the nitty-gritty of testing, you need to make sure that testing is going to do you (and your team) some good.  That means establishing a process.  You need to make sure your code gets tested and that broken builds don't get deployed.

## 1. Sign Up With Awesome Services
Using all of these services will make your life easier and ensure that the tests get written and run.  Because if no one is running your tests, there is no point in writing them.

### Github
The other services listed below won't be _as_ useful for you if you aren't hosting your code on Github.  The reason for this that Github has great APIs which let these services automatically attach information to your codebase.

### Continuous Testing
You should put a barrier between you and your deploys by using a continuous deployment service.  These services watch your branches, and automatically run your tests against each branch.  When a branch is passing, they tag that branch (on Github) with a note saying "This branch passes." This is **very** helpful in code review.

They also test `master` and can automatically deploy new commits from it when all the tests are green. The idea is, **never deploy manually.** Let your testing service do that for you only when all your tests pass.

Using one of these services will ensure that a) all your tests get run, and b) you never ship broken code.  

I've seen two main options around:

1. [Semaphore](https://semaphoreapp.com/)
2. [RailsOnFire](https://www.railsonfire.com/)

### Automated Code Review with [Code Climate](https://codeclimate.com/)
Code Climate automatically checks your ruby code for any duplication, code smells and complexity.  It grades your apps and every class in your apps with a letter grade, giving you great insight into what code should get refactored.

## 2. Establish a Code Review Process
Your testing won't do much good if no one is making sure that the tests are passing.  You need a code review process, and I suggest the following:

### Branches
**Features**: All major features are developed in branches, prefixed with `feature_`.  When a feature is done, the owner of the branch should create a pull request from that branch into `master`.

**Bugs**: Tiny bugs can be fixed on `master`. Every reasonably big bug should be: 

1. Recorded in your code host's issue tracker (Github's is really nice)
2. Fixed in a new branch titled `bugfix_issue_234` (where 234 is the number of the issue)
3. Every commit should include "#234" in the title.  This associates the commit with the issue.
4. When the bug is fixed, a pull request should be opened into `master`.

### Meaningful Code Review
Assign one of your team members per project to be responsible for code review.  They are responsible to:

1. Merge pull requests into master, **after**
2. Examining the code for quality ([Code Climate](http://codeclimate.com/) can help with this)
3. Make sure the tests are passing (Automated services can do this for you, see above)
4. Deploy the code.  (Automated services can also do this for you)

In a perfect world, **ONLY** this team member is allowed to merge pull requests.  This can be accomplished with permissions on the repo.

## 3. Read These Articles

* [My new favorite rspec pattern](http://blog.thefrontiergroup.com.au/2012/06/my-new-favourite-rspec-pattern/)
* [My rspec tips](https://www.evernote.com/shard/s19/sh/9e13bd34-5db5-49b1-aa5c-03fc8310d88e/bb656f5793aee5591a36c87b5ff7ec8a) - use the `let` syntax more than this guy.

More articles on the way.