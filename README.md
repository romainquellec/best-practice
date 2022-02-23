# best-practices

Pragmatic recommendations based on clean coding principles for teams working on TypeScript/JavaScript web application.

# Big principles:
## Programming is a social activity, source code (with tests) is communication and documentation
Code is written for computer to run, and also for people who read, change, and maintain the codebase. The code must conveys its purpose, its internal logic, inputs and outputs to people.

## Attention is the most precious resource in a project
Writing software requires long periods of uninterrupted concentration.

## Simplicity is the best ally in the long run
Avoid accidental complexity as much as possible, and embrace the domain complexity. Don't fall for hype technology or architecture. Only write code you need today (YAGNI). Run with the minimum that achieves the business goals.

# Architecture
![Clean Architecture](https://khalilstemmler.com/img/blog/software-architecture-design/app-logic-layers.svg)
## Domain Layer
Responsibility: encapsulate business entities and core rules. It represents the business entity shapes as classes or types.

## Application Layer
Responsibility: encapsulate application-specific use cases and rules. It should be highly declarative and read like a recipe. Use dependency injection (Adapter Logic) to use the infrastructure layer (to load and persist entity data, email sending service, etc.)

## Intrastructure Details
Persistence modules do not create entities, that is the role of Services. Looking at the interface should reveal nothing about the technology behind the storage layer. Add an explicit API interface type to your codebase. This way, different implementations can exist at the same time. A "real" one can save to the database of choice, another to memory for testing

# Testing
The purpose of testing is to ensure the product works according to its business purposes. It must work even after adding or removing functionality or refactoring the codebase.

Testing is crucial. It is also a convoluted topic and can quickly turn into a counterproductive time sink.

Clean Architecture allows to test fastly and easily use cases, so it should represent 90% of the tests.
10 other percent is integration and end-to-end tests.

# Misc

## Naming (is hard)
Name things as they are in the business language. Use descriptive names for types, classes, variables, etc.: they should be short, to the point and precise. 

## Some simple rules :
- 1 project = 1 repo
- start with modular monolith

## Declarative programming
How do we write code that others understand easily? By stating our intentions in short, abstract, human language-like expressions and moving the technical details into lower-level modules.

Consider the following code snippet. It is obvious what's supposed to happen without seeing the implementation details, and it's also clear where to look to understand more.

    const buyTicket = async ({repository: Repository}, {userId: UserId, concertId: ConcertId}): Promise<Ticket> => {
        const [concert, user] = await Promise.all([
            repository.getConcert(concertId),
            repository.getUser(userId)
        ])
    
        if (!ticketsAvailableFor(concert)) {
            throw new TicketsNotAvailableError()
        }
    
        if (!hasFundsFor(user, concert)) {
            throw new NotEnoughFundsError()
        }
    
        const ticket = new Ticket(concert, user)
    
        await repository.recordTransaction(user, ticket)
    
        return ticket
    }

## Readme
What to include in a README?
 - Instructions to get the project working in a dev environment so people can start being productive fast
 - Brief notes on quirks, known issues, peculiar things in the project that would surprise others

## Folder Structure
Try to follow the Clean Architecture as much as you can.

## Code formatting
Avoid endless debates about formatting questions. Use Prettier and eslint, and check before delivering to the codebase.

## Code comments
You don't need comments in the code. Use declarative code.

## Logging
Hide the dependency behind a local proxy, so you can change the implementation as you see fit.

## Git
Be pragmatic; find a productive workflow. Source control should be a tool, not a chore.

Use the master branch to deploy into different environments and use feature branches to prepare PRs.

Resist having any long-running branches apart from master: they are the source of confusion and frustration. The branch list should reveal the current work effort (not the past). Long-running branches get out of sync quickly anyway.

No change can enter the master (production) branch without a PR.

Create small PRs: they are easier to create, so the code quality is better, and they are easier to review, so more mistakes will be caught.

Use feature branches that live at most two days: longer ones either get too big (impossible to review) or will introduce merge conflicts.

To help code reviews make sure the feature branch history is readable.

Use squash when merging PRs:

-   it produces clean git history
-   requires no extra effort from the developer
-   flattens the PR into a single commit, hiding individual commits you made during the PR

Other merge strategies either result in a messy history or require the developer to manually tinker with git or prepare each commit with great care. That amount of effort is always better spent elsewhere.

When PRs are small, the individual commits in the feature branch should not matter: what matters is the result, the complete change that goes into master.

Make sure merged feature branches are deleted, keep a tidy work environment.

Do not keep deprecated code in HEAD: delete it, you can always go back and find it.

# Process
tl;dr : Use ATDD
