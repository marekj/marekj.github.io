---
layout: post
title: Learn Rubik's Cube with rubytester's heuristic move notation
---

I find official Rubik's cube move notation confusing. I get lost with algorithms like `U R U' R' U' F' U F`. The congitive load is a bit too heavy for me.

I tried to map the moves to some visual vocabulary that makes sense to me, I needed a visual scaffolding system to learn how to solve the Rubik's cube. I came up with heuristic moves base on manipulating everyday objects. It took me few days of practice and I can now solve the cube in about 2 minutes. I don't need to think of those moves any more, it has become muscle memory at this point but I think for a novice there has to be a better way than the official one letter designation for a move. It simply sucks unless you have huge determination. I hope that this visual vivid vocabulary can help you too.

### Cognitive Ease Matters

I wanted to come up with sequence notation based on cogntive ease of learning in the presence of disorientation. I converted each official move into a huristic based on moving everyday objects and composed the moves into stanzas; two or three moves per line.

Compare the 3 representations of sequence to follow in order to solve one stage of a cube:

The 8 move official notation in one freaking line::

    U' F' U F U R U' R'

The raw mapping to heuristc moves (a bit more human readable):

    open past close future close drop open lift

And the composition that breaks the linear sequence into stanzas:

    open past
    close future
    close drop
    open lift

I hope you can experience the cognitive ease with which your mind consumes 4 stanzas of two moves each. Imaging speakint it out loud like a chant.

My argument for developing these moves is that congnitive ease matters when learning in the presence of disorientation and I wanted to test my assumption that I can learn anything if the learning is designed as a systematic skill aquisition in the presence of disorientation. I guarantee you will be able to quickly learn to solve the rubik's cube this way. As a kid I never learned to solve Rubik's Cube. As an adult I was frustrated but a couple of months ago I decided to confront the Myth of the Impossible and approach it in a new way. So hang out with your Initial Disorientation and have luck learning.

## Working with Official Instructions

I used the initial instructions that came with the cube I bought from ShengShou: [I scanned them for you here](https://www.dropbox.com/s/lpfmzd4aof18ryk/rubik_china.pdf?dl=0). You should print out these 2 pages of instructions and follow along. Print in Color. If you don't have a color printer just follow along on your screen.

I find the ShengShou instructions simpler than the [official solution guide (pdf)](http://rubiks.com/uploads/general_content/Rubiks_cube_3x3_solution-en.pdf). ShengShou instructions never use the L side nor the D side of the cube. That's 2 sides out of 6 you don't have to worry about.

Notice the instructions use the official notation. I am too lazy to make a video or an instruction cheet sheet (maybe later) but for now I'll try to capture what worked for me in words while you examine the printouts.

Before you proceed remember the most important thing to remember in all the move is: What Color is your Face... you will see what I mean when you read instructions.

## How to hold the cube in your hand:

I am right-handed so these moves are biased towards right-handed people. The visual vocabulary is also geared towards right-handed people. There are 2 main ways to hold a cube for these moves.

Hand Position 1: Hold the cube in your left hand, between the thumb and the ring finger holding the middle pieces with the thumb facing you.

This side of the Cube is called a (F) side, your FACE side. Your ring finger is holding the (B)ack side.

Your Face has a color, it's the color of the middle piece under your left thumb facing you.

All the the moves are performed in the context of your FACE color. What Color is your Face will be the prominent way of orienting yourself when making sequence moves.

You make all the moves with your right hand (left hand's fingers assisting in the move). Each side moves clockwise or counterclockwise direction. Clockwise being the Future on the clock and Counterclockwise moving time back into the Past. In position 1 you move U and R sides (4 moves)

Hand Position 1+ (Pinky version): While in Position 1 your left index finger is resting on on top U and your pinky holding the D side.

This allows you to let go of your thumb so you can move (F)ace side with your right hand (while cube rests its D side on your left pinky finger). In this position you move F side (2 moves)

Hand Position 2: Hold the cube in your left hand like before but this time the thumb is on the bottom side (D) and ring finger on the top side (U).

Hold it like you are examining a gold coin. This position allows you to freely move (F)ront side and (B)ack side with your right hand without the help of index/pinky finger support. In this position 2 you move F, B and R sides (6 moves). This position is used in one sequence in Stage 6.

## Cube's Side heuristc mapping:

Here is a mapping I use for official moves. It's a heuristc based on moving everyday objects.

- (F)ront (F)ace side:
    - `F` and `F'` => `Future` and `Past`
    - Heuristc: The Face of a clock. You move clock forward into the `Future`. You move it back to the `Past`
        - In Position 2: you are free to turn clockwise and counterclockwise with your righ index finger and a thumb. Future and Past.
        - In Position 1 plus Pinky: You loosen left thumb's hold on F and let the Pinky to hold the cube from falling out of your hands while you use right index finger and thumb to move clockwise and counterclockwise. Future and Past.
- (R)ight side:
    - `R` and `R'` => `Drop` and `Lift`
    - Heuristic: you drop a corner piece and then you lift it. Think of the Right face as a Crane you operate. you drop items down, you lift them up.
        - In Position 1 and 2: You Drop and Lift.
- (B)ack side:
    - `B` and `B'` => `Tighen` and `Loosen`
    - Heuristic: `Rigthy Tighty` and `Lefty Loosy`. Raching beind the turning on the nut of a bolt. Tighten is clockwise, Loosen is counterclockwise
        - In Position 2: the only position to hold the cube you reach beghind and you righty tighty and lefty loosy the B side.
- (U)p side:
    - `U` and `U'` => `Close` and `Open`
    - Heurisic: Top of a bottle or a jar. Natural hand movement to `close` and `open` a top of the jar or a bottle cap.
        - In Position 1: Close the bottle cap for clockwise move and Open the bottle cap for counterclockwise move.

That's all the 8 moves needed to construct sequences to solve the cube. I don't use (L)eft nor (D)own side in solving the cube however here are some heuristics I thought I needed when I first started.:

- (L)eft side
    - `L` and `L'` => `Reveal` and `Hide` (I don't use this move in solving the cube. No need to learn these)
    - Heuristc: you turn clockwise to reveal what's in the back, you move counterclockwise to hide visible top side.
- (D)own side:
    - `D` and `D'` => `Cap` and `Drain` (I don't use this move in solving the cube. No need to learn these)
    - Heuristic: Imagine changing oil on a car or opening a cap at the bottom of some kind of a bottle. You `Cap` clockwise to close the leak but `Drain` counterclockwise to let the fluid leak. (it's the oppposite of a bottle cap)

## SEQUENCES: Algorithms for solving the Cube

There are 7 stages to solving the cube. You complete each stage at a time. Each stage gives you one or more sequences to complete. Sequences are the stanza based moves that incorporate the names of moves. The move names automatically imply which side you move. Some stanzas are two or three moves. The reason is to combine similary hand movements into a short mini gesteure sequence. Some stanzas have an empty line between them. This is a place for you to breathe before performing the next mini gesture sequence.

As you move the side speak the move name out loud. Combine one or two moves into a stanza and speak the stanzas out loud like a chant as your hand perofrms the move. Treat the names organized into stanzas as a scaffolding you erect around building a skill. You begin with scaffolding which helps you build a skill. As the skill becomes solid you discard the scaffolding.

### Stage 1: Solve the White Cross

Position: Hold cube in Position 1. The instructions tell you what White side is Down (and Yellow is UP).

I practiced with White side UP. (at the same time Yellow side is Down). Your face can be any of the other 4 colors.

This part is pure struggle. The official guide simply states: "You should be able to do this by yourself without needing algorithms" but that gets people stuck. If you can struggle and accomplish this you will learn a great deal about how the cube moves work in 3D.

Hints:
- You need to find 4 edge pieces White and other 4 colors (There is No White/Yellow edge piece of course).
- When you find the piece anywhere you should have 2 to 3 moves to move it to its proper location.
- Think in 3D. Experiment. Struggle.
- Think "if the piece needs to move from here to there then where does it need to be first"

### Stage 2: Solve the White Corners

Position: Hold the cube in Position 1. Turn the cube so Yellow is (U)p and White is (D)own. Next set of algorithms is to "push" so to speak the white corner pieces down to the bottom.

When: White on F side and Face color on U side (with R color matching middle cube on R)

official:

    past open future

my preffered version:

    close drop
    open lift

Why my preffered version? I make one extra move but my hand is in position 1. In official version I find my had to be in postion 1+ pinky and my fingers ran into each other. One extra move solve the uncomfortable way of holding the cube.

When: White on R side and Face color on F side (with U color matching middle cube on R)

    drop close
    lift

When: White on U side (with R side matching F and F side matching R)

=> You need to reposition to White on R side position first

    drop close close
    lift open

=> Followed by:

    drop close
    lift

### Stage 3: Solve the Middle layer

Your goal: middle edges in place.

Look for a matching edge piece on the U layer: When the Face color matches the middle you must move the Up color either left or right.

When Going Right:

    close drop
    open lift
    open past
    close future


When Going Left: Change Left side to become your FACE. Your desired edge piece is now on R side (really, see the official instructions)

    open past
    close future
    close drop
    open lift

Notice these are the same moves but two top stanzas of the last move are the last two stanzas of the previous move. Plus you position the cube differently in front of you.

### Stage 4: Solve the Yellow Cross

Your goal: make yellow cross.

There are 3 possible starting positions unless the yellow cross just happened to form by itself when you were solving the middle layer:

Select your FACE to match the one of the yellow positions and: Position 1+ Pinky helps in this algorithm when making the future and past moves.

    future drop close
    lift open past

Notice you may need to repeat this move 2 or 3 more times with each time realign the FACE to the yellow starting positions

### Stage 5: All Yellow Top:

There are 3 starting positions:
- do you have 2 yellow corners? then position yellow as left corner with yellow facing you
- do you have 1 yellow corner? then position yellow corner as left corner (obviously yellow side UP)
- you don't have any yellow corner: then position yellow as left corner with yellow on L side

    drop close
    lift close
    drop close close
    lift

Notice you may need to repeat this sequence 2 or 3 times with each times repositioning top layer to match the starting positions

### Stage 6: Position Yellow Corners:

Now you have all the yellow pieces on top and you need to correctly position the yellow corners.

Hand Position 2: Hold the cube in Position 2 with correctly placed corners facing you. This sequence switches two corners on B side.

    drop loosen drop
    future future

    lift tighten drop
    future future

    lift lift

### Stage 7: Position Yellow Edges:

Hand Position 1: When one of the edges is in correct position then realign the cube to have the solved edge on the B side.

    drop open
    drop close drop close
    drop open

    lift open
    lift lift

Notice you may need to repeat this sequence 2 or 3 times to correctly rotate the edge pieces to their respective locations.

### Solved:

I presented this way of solving Rubik's cube at [Test Automation Bazaar 2015 in Austin, TX](http://watir.com/2014/12/16/announcing-test-automation-bazaar-jan-16-17-in-austin/)

Major weakness in this post: some visual drawings would be nice but I don't have time. Some youtube videos would be nice but I don't have time for that. Print out the ShengShou solution and follow along. Let me know if this helps.
