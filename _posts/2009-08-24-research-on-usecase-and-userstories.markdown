--- 
title:      Research on Usecase and Userstories
layout: post
---

## To distinguish 'Simulation' from 'Verification' strategies in acceptance testing code.

I chanced upon a compendium of XP 2004 papers and there I've found a paper by Johan Andersson and Geoff Bache
[Adventures in GUI Acceptance Testing](http://springerlink.metapress.com/content/nl1749aayqdfhlqy/?p=5d1e90c17c4b4e92ab301dc1106ce9c1&pi=0)
I love the concept of 'simulation' and 'verification' as two different GUI test strategies.
'Simulation' is all about driving the GUI and 'verification' is all about assertions.

This is how I have been structuring my GUI Acceptance Testing with Watir.
I refactor all the code that 'simulates' user's scenarios into code I call UseCase (Scenarios, Workflow)
and then I write test code (Rspec or TestUnit) that is responsible for 'verification'.
This separation allows me to drive GUI to setup context in which 'checking' and 'verification' occurs.
Since my usecase is a model driven by examples
(notetoself: find article on Model -> Example -> Model feedback cycle to refine model by examples).

Unfortunately that XP 2004 paper is "behind the moneywall" on Springer but
[you can find the docs here](http://texttest.carmen.se/index.php?page=publications) and [here is a googlebook text](http://books.google.com/books?id=Zr4mJLaCIQAC&lpg=PA180&ots=Go1Y-DrL5k&dq=Johan%20Andersson%20%20Geoff%20Bache&pg=PA180#v=onepage&q=Johan%20Andersson%20%20Geoff%20Bache&f=false)
In XP 2005 they continue with
[Further Adventures in Acceptance Testing](http://www.springerlink.com/content/mef1w2bydp8mvuxj/) talking about xUseCase).

The concepts from these guys are interesting.
It seems I have been doing something similar in my acceptance testing approach
and it is really great to read some feedback on such an approach.

The main idea I see here is the distinction between Usage of Simulation code and Test Verification code
where Usage Simulation code is written in a customer's language, a domain language customer operates to produce results in that domain to reflect the user's intent.
This reminds me of Usage-First code design strategy.

## Usage-First design strategy

I like the way TDD invites you to write code. You "write the code you wish you had", you pretend that it's there,
you write how a client uses something, and then you write that something.
This is how I write UseCase scenarios in code. It's in a style of Red, Green, Refactor.

Very important point. UPDATE: Bret Pettichord mentioned at LSRC that we could call this "Subject Oriented Development".
I am not sure how that fits but again, important point becuase in BDD we are designing behaviour of objects
but in Subject Oriented Development you design system behaving according to the Subject. (more on that at some point)

OK, so a couple of quotes from the those guys' papers.

> Acceptance Tests are entirely independent of design, because they do not interact with it. This means they form a solid rock to lean on when doing refactoring (__of the design__, emp. added)

> Unit Test is a design statement. Acceptance Test is a statement of a correct behaviour of the system. Unit Tests act as design documentation, acceptance tests document system behaviour. (paraphrased)


## Why UseCase and not a UserStory

Read Mike Cohn's paper on
[Advantages of User Stories for Requirements October 2004 published in InformIT Network](http://www.mountaingoatsoftware.com/articles/27-advantages-of-user-stories-for-requirements)
He explains how UserStories differ from UseCases. My takeaway is UserStories are used in BDD but I would use UseCases in Subject Oriented Testing becuase it follows Example > Model > Example cycle and has persistence of End to End goal seeking. 

These are some good distinctions I think. Any feedback?

