# Evaluating Game Mechanics For Depth 评估游戏机制的深度

原文地址：[https://www.gamasutra.com/view/feature/134273/evaluating_game_mechanics_for_depth.php?print=1](https://www.gamasutra.com/view/feature/134273/evaluating_game_mechanics_for_depth.php?print=1)

By Mike Stout

[Former Insomniac designer Mike Stout takes shares a useful rubric for judging the depth of play mechanics, including checks for redundant ones, in this in-depth design article, which contains examples from the Ratchet & Clank series.]

> 前 Insomniac 设计师 Mike Stout 分享了一个评判游戏机制深度，也包括检查冗余，的有效准则。并在在这篇深入的设计文章中纳入了来自 Ratchet & Clank 系列的例子。

## The Problem 问题

Often, in game development, a design that looks great on paper doesn't turn out as well in practice as you'd hoped. It comes across as "shallow" or "flat." Perhaps play-testers, publishers, or peers describe it as "needing more variety" or as "feeling repetitive."

在游戏开发中，那些在纸面上看起来非常宏伟的设计结果往往并没有你希望的那么好。它给人们的印象是“浅”或“平”。可能游戏测试人员、发行商或者同行会把它描述为“需要更多的变化”或者“感觉重复”。

Every game designer has heard these complaints at one time or another. I've bumped up against problems like this on every game I've ever worked on and there are three ways I like to approach solving them.

每一个游戏设计师都会在某个时候听到这些抱怨。在我开发过的每一款游戏中我都遇到了这样的问题，我喜欢用三种方法来解决它们。

If the players felt the game overall didn't have enough variety you can add more game mechanics to the game. Think of this as increasing the game's "breadth."

如果玩家觉得游戏整体上缺乏足够的多样性，你可以添加更多的游戏机制到游戏中。可以将此视为增加游戏的“广度”。

- Buzzwords to watch for: The game is "a one-trick pony," "repetitive," "or needs more variety."

- 关注这些关键词：这款游戏是“只有这一点”、“重复的”或者“需要更多的变化”。

- Feedback that can be fixed with these kind of content expansions tends to describe the game as a whole. Players feel they don't have enough different things to do on a global level.

- 通过这种内容扩张可以处理的反馈，都趋向于把这个游戏描述为一个整体。玩家感觉到他们在完成整个关卡时没有足够多不同的事情要做。

If players feel that an individual game mechanic is flat and unrewarding you can refine that mechanic's "theatrics" by giving the player better feedback, more rewards, better effects, cooler sounds, more personality, a cooler camera, or other bells and whistles. After theatrics refinements, players will often -- with no changes to the underlying gameplay -- tell you the problem is fixed.

如果玩家觉得某个游戏机制平淡无奇且没什么收获，你可以重新定义这个机制的“舞台效果”，通过给用户更多反馈，更多的奖励，更好的效果，更酷的声音，更多的人物个性，更酷的镜头，或者其他华丽的点缀。在舞台效果改进以后，玩家通常将会告诉你问题已经解决了——而基本的游戏设置并没有改变。

- Buzzwords to watch for: A given game mechanic is "boring," "repetitive," or "just not fun."

- 关注这些关键词：给出的游戏机制是“无聊的”、“重复的”或者“只是不好玩”。

- Feedback that can be fixed with theatrics improvements usually describes a single game mechanic, but is vague and "touchy-feely."

- 通过舞台效果改进就能处理的反馈，通常会单独描述游戏机制，但是通常是含糊的和“感性的”。

If players feel that an individual game mechanic "isn't giving them a good enough challenge," or feel that "the mechanic is fun at first but gets old quickly," you need to add depth to your mechanics.

如果玩家觉得某个游戏机制“并没有给他们足够好的挑战”，或者觉得“这个机制开始很好玩但很快就过去了”，你就需要给你的机制增加深度。

- Buzzwords to watch for: A given game mechanic is "too shallow," "too easy," or "flat." Often players will say the mechanic started out fun, but that it quickly got repetitive or boring.

- 关注这些关键词：给出的游戏机制“太浅了”，“太简单”或者“很平淡”。通常玩家会说这个机制开始好玩，但是很快变得重复或无聊。

- It's a good idea to pump up the theatrics when you get feedback like this, but while it might help players tolerate a mechanic for longer, it will only go so far. When theatrics fail, it's time to knuckle down, roll up your sleeves, and get to work on making your game mechanic deeper.

- 当你收到像这样的反馈时，提升舞台效果也是一个好主意，但是它可能只是让玩家忍受这个机制更长的时间，它也就只能走这么远了。当舞台效果失败了，是时候该弯下腰，卷起袖子，开始让你的游戏机制更有深度了。

While each of these could be the focus of its own article, this article will focus on perhaps the trickiest of the three: depth. By the time you're done reading this article, I hope to describe for you the tools I use to find out why a game mechanic doesn't feel deep enough. Even better, I hope to impart a few techniques that I've found help a lot when it comes time to fix a shallow game mechanic.

当然上面的每一个点都可以成为一篇文章的重点，而本文将关注三者中最棘手的一个：深度。当你在这篇文章时，我希望为你介绍我用来找出为什么会觉得一个游戏机制不够深的工具。甚至更好的情况是，我希望传授一些技巧，我发现这些技巧在处理一个肤浅的游戏机制时都非常有帮助。

## First: A Few Definitions 首先：一些定义

Before diving into a discussion on depth, I wanted to define a couple of terms that I'll be using a lot in this article:

在进入关于深度的深入讨论之前，我想先定义一些术语，我将在这篇文章中大量的使用到它们：

**Game Mechanic**: When I say "game mechanic" I'm referring to any major chunk of gameplay in a video game. Using the classic _The Legend of Zelda: A Link to the Past_ as an example, here are a batch of game mechanics: sword combat, block pushing, boomerang throwing, swimming, button-based puzzles, hazard-avoidance, use of specific weapons, etc...

**游戏机制**：当我说“游戏机制”时，我指的是在一个电子游戏中的主要核心内容和玩法。用经典的《_The Legend of Zelda: A Link to the Past_》游戏举例，它有一些游戏机制：剑战，推块，扔飞镖，游泳，按键解谜，避险，特殊武器的使用，等等……

**Challenge**: A challenge is any in-game scenario that tests the player's skill at a specific game mechanic. An example of this would be an individual room in a Zelda dungeon, a grindrail segment in Ratchet & Clank, or a combat encounter in Halo.

**挑战**：挑战是在特定的游戏机制中测试玩家的技能的任何游戏内置场景。关于此的例子可能是 Zelda 地牢中的一个独立房间，Ratchet & Clank 中的一个窄道片段，或者 Halo 中的一次遭遇战。

## So What Does "Deep" Mean, Anyway? “深度”到底是什么意思？ 
Dictionary.com defines depth as: "The amount of knowledge, intelligence, wisdom, insight, feeling... evident either in some product of the mind, as a learned paper, argument, work of art, etc." As is evident from the scope of this definition, depth can be an incredibly personal term, and can mean a lot of different things to a lot of different people.

Dictionary.com 把深度定义为：“知识、智力、智慧、洞察力、感知力……的数量，显著的出现在大脑的产物中，如学术论文、辩论、艺术品等等。”从这个定义的范围可以明显看出，深度可以是一个极其个人的术语，对于许多不同的人来说可能意味着许多不同的东西。

To me, it describes a sweet spot -- that point during a game where the player can repeatedly display his mastery of a game mechanic. Challenges never stay the same long enough to be boring and yet they also don't change so fast that the player can't enjoy his mastery over the game.

对于我来说，它描述了一个兴奋点——玩家可以在游戏中的这个点反复的展示他对游戏机制的掌握。挑战永远不会固定不变太长的时间而变得无聊，然而也不会变得那么快以致于玩家无法享受他对游戏的掌控。

Clearly this "depth" is something we want to achieve in many of our mechanics, but it's often less clear how to obtain it.

显然，“深度”是我们在许多机制中都想要达到的，但是却很少清楚如何获得它。

In my experience, in order for a game mechanic to be deep it needs two very important things:

以我的经验来看，想要让一个游戏机制变得有深度，有两件事情非常重要：

- It needs clear objectives, so the player knows what he has to do to succeed. Confusion and obfuscation tend to make players feel like a mechanic is LESS deep once they find themselves needing to experiment randomly to win.

- 需要清晰的目标，那么玩家就知道他要做什么才能赢。一旦玩家发现他们需要通过随机试验才能赢，混乱和迷惑会趋向于使他们觉得这个机制是**缺乏**深度的。

- It needs a variety of **Meaningful Skills** that you, as a game designer, can use to create good challenges for the player and that the player in turn can use to achieve mastery over the game.

- 需要各种各样的**有意义的技能**，作为游戏设计师，你可以用来为玩家设计好的挑战，而玩家也可以用来获得对整个游戏的掌控。


## Objectives 目标

When a player enters a challenge, he must have a good idea of what his objectives are. Another good way to put this is to say that he must be able to clearly visualize the completion state of the challenge.

当玩家进入一个挑战，他必须清楚他的目标是什么。另一个很好的说法是，他必须能够清楚的看到这个挑战的完成状态。

In _The Legend of Zelda: A Link to the Past_, when players see a door that looks like this, they know they must find a special "Boss Key" to go through. These doors are a good, though simple, example of an objective. Once he sees the door, the player knows he needs to find the key and bring it back.

在《_The Legend of Zelda: A Link to the Past_》中，当玩家看到一个像这样的门，他们就知道他们必须找到一个特殊的“Boss Key”才能通过。这些门就是目标的很好的例子，尽管很简单。一旦他看到门，玩家就知道他需要找到钥匙并带回来。

Clear objectives are a must if you want to create depth in your game mechanic. As I mentioned before, if the player doesn't know what the completion state of the challenge should be, he's reduced to floundering about and trying things randomly instead of demonstrating his mastery of the game's mechanics.

如果你想要在你的游戏机制中创造深度，清晰的目标是必要条件。正如我之前提到的，如果玩家不知道挑战的完成状态应该是什么，他就会陷入挣扎和随机尝试，而不是展示他对游戏机制的掌握。

## Meaningful Skills 有意义的技能

Meaningful Skills are all the "things" the player must do to take a challenge from its beginning state to its completion state. In other words, once the player has recognized a challenge's objective, the work has only begun.

有意义的技能指的是从一个挑战的开始状态到完成状态之间玩家必须要做的所有“事情”。换句话说，一旦玩家意识到挑战的目标，这个工作就已经开始了。

It can't be stressed enough that I'm referring to meaningful skills. "Meaningful" is an incredibly important part of this equation. If a skill is too basic, it will not help make your mechanic feel deeper. At that point, it becomes a simple task the player must complete, like checking items off a shopping list.

对于我正提及的有意义的技能，是怎么强调都不过分的。“有意义”是这个公式中极其重要的部分。如果一个技能太基础，它将无助于让你的机制变得更有深度。在那个时候，它就变成了一件玩家必须完成的任务，就像从购物清单上检查项目一样。

A classic example of a too-basic skill can be found in the statement "move from point A to point B." That kind of fundamental movement challenge is essentially the same as saying "hold a button on the controller to win." It's foundational, but it's too simple. It's not deep.

一个经典的太基础的技能的例子可以在这句话中找到，“从A点移动到B点”。这种基本移动挑战基本上和说“按住控制器上的按钮就可以赢”是一样的。这是基础，但是太简单的。它并没有深度。

Further, when you really think about it, when you say "move from point A to point B," you're actually talking more about the objective of a challenge and not the skill required to achieve the objective.

此外，若你认真思考这件事情时你会发现，当你说“从A点移动到B点”时，你实际上更多的是谈论挑战的目标，而不是达到目标所需的技能。

This sounds like a small distinction, but is in fact **very** important. Making this shift in how you think about designing game mechanics allows you to see depth-related problems that you otherwise would not.

这听起来是一个很小的区别，但实际上**非常的**重要。在设计游戏机制的思维方式上进行这种转变，可以让你看到与深度相关的问题，否则你不会看到。

## Meaningful Skills: A Morality Tale 有意义的技能：良心的故事

When I was a junior designer on _Ratchet & Clank 2_, I was given the task of coming up with all the "Tractor Beam" puzzles for the two levels of the game that used them. The tractor beam was a game mechanic that allowed Ratchet to freely move large objects marked with a special tractor beam symbol. Essentially this is a theatrical "paint-over" of classic block-pushing challenges from games like _The Legend of Zelda_.

当我还在《_Ratchet & Clank 2_》做初级设计师时，我分到的任务是为两关游戏设计出所有的“牵引光束”谜题。牵引光束是一种游戏机制，可以让 Ratchet 自由的移动标记了特殊牵引光束符号的大物体。从本质上说，这是来自于像《_The Legend of Zelda_》这样的游戏的经典推块挑战的戏剧化“上色”。

Coming up with the training setups was easy. The player simply had to move blocks from point A to point B so that he could jump up on them and get out of a pit. My problems began, however, when I tried to come up with more advanced challenges. I quickly came to an impasse. Moving blocks from one place to another, I saw, was too basic a skill. Beyond the training example, there wasn't much more I could do to "ramp it up" and provide varied, escalating challenges.

制定训练步骤很容易。玩家只需将木块从A点移动到B点，这样他就可以跳到木块上并从坑里出来。然而，当我试图提出更高级的挑战时，我的问题就开始了。我很快陷入了绝境。把木块从一个地方移动到另一个地方，在我看来，是一个太基础的技能。除了培训的例子之外，我没有更多的办法来“提高”，以及提供各种各样的、不断升级的挑战。

At this time in my career, I didn't yet understand the important distinction between meaningful skills and too-basic skills. I didn't know how important clearly identifiable objectives were. And so, lacking experience, I decided to just start adding features until the mechanic was deep enough. If you're groaning at this, then I congratulate you. I'm groaning, myself, as I write this.

在我职业生涯的这个阶段，我还没有理解有意义的技能和太基础的技能之间的重要区别。我不知道明确的目标有多重要。因此，由于缺乏经验，我决定开始添加功能，直到这个机制有足够的深度。如果你正为此而叹息，那么恭喜你。当我写这篇文章时，我自己也正在叹气。

Besides moving blocks around, I decided it would be great if the player could grab and drag around a wacky robot (if you're reading this and thinking "oh, you improved the theatrics," you get a cookie). The player could then drop the robot on buttons to open doors. This didn't help as much as I wanted it to -- it just still seemed way to shallow.

除了移动木块，我决定如果玩家能够抓住和拖着一个古怪的机器人那就会非常好（如果你读到这里，就想到“啊哦，你改进了舞台效果”， 那你可以得到一个奖励）。然后玩家可以把机器人放到按钮上就能打开门。这并没有像我所希望的那样有多大的帮助——它看起来还是太肤浅了。

So I forged ahead and kept adding features (groan).

因为我奋勇向前，继续添加功能（叹气）。

I added bombs that you could drag around to blow up doors and little energy slingshots you could use to fling the bombs around at targets.

我添加了炸弹，你可以拖到周围来炸开门，以及一些小能量的弹弓，你可以用来把炸弹投射到目标周围。

I decided blocks would be able to get in the way of laser beams so you could get past them.

我决定木块可以挡住激光，这样你就可以越过它们。

I added special blocks with explosive rockets inside that would blow up certain doors.

我加了一个里面装有爆炸性火箭的特殊木块，可以炸开某些门。

Finally, I made it so that some of the blocks slid around inside a groove on the floor. The player had to figure out how to arrange the blocks into a specific order, but the blocks all got into each others way, since they shared a groove.

最后，我让一些木块在地面上的滑槽里滑动。玩家们必须想出如何把这些木块排列成一个特定的顺序，但是木块都是相互勾连在一起的，因为他们共享一个滑槽。

By the end of all that adding, not only was I permanently on several programmers' hate lists, but the tractor beam game mechanic was quickly getting too bloated. It was way too complicated for players to handle. Playtests were generating feedback that players were routinely confused. We had to devote about half the time spent with the tractor beam purely to training, so that our players could understand the basic gist of what they needed to do.

在添加了所有这些以后，我不仅永远的存在于了几个程序员的仇恨名单上，牵引光束游戏机制也很快变得臃肿。这对于玩家来说太复杂了。游戏测试员反馈说，玩家经常感到困惑。我们必须把几乎一半是时间单纯的花费在牵引光束的训练上，这样玩家才能够理解他们要做的基本要点。

Looking back, it's clear to me why the mechanic never felt deep enough: I kept adding new objectives, but failed to add many meaningful skills.

回过头来看，我就很清楚为什么机制一直没有感到足够有深度了：我一直在添加新的目标，而没有添加一点有意义的技能。

I'll go into this a little more below, but I bring it up now because it illustrates how meaningful skills contribute much more to a feeling of depth than objectives do.

我将在下面更深入地讨论这一点，但我现在提出它，因为它说明了有意义的技能比目标对深度感的贡献大得多。

Experiences like the one I had with the tractor beam taught me a valuable lesson: most game mechanics that don't feel deep enough feel that way because they have too many objectives and not enough meaningful skills.

像这样在牵引光束上的经历给我上来非常有价值的一课：大多数感觉不够深度的游戏机制都有这种感觉，因为它们有太多的目标，而没有足够的有意义的技能。

While players found the Inspector Bot wacky and funny, adding him did not succeed at the goal of making the Tractor Beam game mechanic deeper.

虽然玩家觉得巡逻机器人古怪有趣，但是添加它并没有达成让牵引光束游戏机制更有深度的目标。

## Making a Statement 发表声明

So if I could go back in time, how would I help myself make the tractor beam challenge deeper? Well the first thing I'd suggest is that my past-self come up with an "Activity Statement" for each challenge in the segment.

An Activity Statement is a simple sentence that describes a challenge by stating both the objective of a challenge and the meaningful skills that the player must use to obtain his objective.

For example, a simple platforming challenge could be described in an Activity Statement as: "I want the player to jump up to that platform." In this case "jump up" is the meaningful skill and "that platform" is the objective.

A more complex platforming Activity Statement might be "I want the player to double jump straight up and then glide down to that platform" or "I want the player to time his jump to avoid the fire spouts and land on that platform."

The Activity Statements listed above all contain a mixture of objectives and meaningful skills. But what happens if you exclude one of those ingredients? We've established that challenges without a clear objective are not deep, but it's also true that game mechanics that feel shallow tend to include many objectives, but few meaningful skills.

Here is what Activity Statements would have looked like for some of the tractor beam challenges past-me designed:

- "I want the player to move a wacky robot from his starting spot to a button on the floor."
- "I want the player to move a bomb from its starting spot to a spot in front of that door."
- "I want the player to move a block from its starting space so that it blocks that laser beam."
- "I want the player to move an explosive rocket block to that button on the floor."

You'll notice that the above statements all clearly outline objectives, but no meaningful skills. In fact, when you examine them closely, all of them outline the same objective, and it's not even a particularly interesting objective!

Each statement is basically saying "Move from point A to point B," which we already know describes a skill so basic that it doesn't make our game mechanic any deeper.

With the other two challenges past-me designed, it looks like I was onto something a little deeper:

- "I want the player to move a bomb its starting spot into that energy slingshot and use it to blow up a target."
- "I want the player to slide these blocks around inside a groove and arrange them in a specific order."

Both of these Activity Statements, "use the energy slingshot to blow up a target" and "arrange the blocks in a specific order" describe skills that are much more meaningful than the others.

Knowing this, I would advise my past-self to:

1. Cut most of the too-basic skills / objectives described above. They all boil down to "take an object near a door to open the door" and could be accomplished with either the bomb or the rocket-block. Nuke redundancy and simplify things for your players, past-me! Do it now!
1. Examine the two deeper mechanics and try to create more challenges for each of them. Past-me would begin designing these new challenges by writing an Activity Statement for each one.
1. Prototype the new challenges in-game.
1. Play-test the new challenges.
1. If the whole thing is still too shallow, add another Meaningful Skill to the list (make sure it's not an objective!) and repeat from step 1.

The objective of this tractor beam challenge is very clear; get the three blocks into the alcoves that best fit them. Further, the Meaningful Skill (organize the blocks despite them getting in each others' way) is much deeper than in any of the other Tractor Beam challenges.

**Note**: All of my examples thus far have been about puzzle-like gameplay, but this article isn't just about puzzles. All types game mechanics can benefit from this way of thinking.

For example, lets say you have a gun combat mechanic that is feeling shallow. Perhaps this is because "use your gun to kill an enemy" doesn't say anything about the meaningful skill. It is purely a statement of objective. In Ratchet & Clank, our gun combat's Activity Statement was more like "Choose the correct weapon or combination of weapons to kill a group of enemies as efficiently as possible."

By altering the Activity Statement during the design phase to more explicitly encompass the meaningful skill (and thereby altering the underlying mechanic), your whole design will get deeper and more satisfying.

## Avoid Vague Activity Statements 避免含糊不清的活动陈述

Activity Statements are an immensely useful tool, but it's possible for them to get you into trouble. The easiest way to get in this kind of trouble is to create a vague Activity Statement.

For example, here is a simple Activity Statement that could apply to most of the challenges in Portal:

"I want the player to use the portal gun to get this block on top of that button."

While the indicated skill (use the portal gun) in this simple Activity Statement happens to be a Meaningful Skill, this kind of vague statement can get you into trouble if you're not lucky.

The Clank gameplay in the Ratchet & Clank games is a good example of this.

The Clank challenges always bugged many of the designers I knew at Insomniac, myself included. When we designed them, they seemed like they should be deep activities, but they never turned out that way.

We were able to patch them up by giving them a lot of personality and by keeping them fairly simple (theatrics, baby!) but it took many sequels to add enough depth to them to satisfy us. Players seemed to like them, but we thought we could do better.

In the first two Ratchet & Clank games, Clank needed to give commands to his Gadgebots (cute little robots that followed him around) to get past blockades. There were a few different types of blockades, but in the end they all ended up feeling the same. We patched it up with effects and cute animations, but in general they were quite shallow.

The Activity Statement: "I want the player to command his array of Gadgebots to get him past blockades," in the end, is too vague. It doesn't give enough information to tell whether or not the mechanic will deep enough.

Flash forward to Ratchet & Clank Future: A Crack in Time. Clank now has the ability to record his actions at certain points in the level and then have a hologram play those actions back.

This gave way to challenges with very complex Activity Statements like "I want the player to record himself going to that button, which opens a door. Then I want him to play back the recording and, once the hologram hits the button and the door opens, I want him to go through the door." You'll notice clear objectives "go to the button to open the door" and "go through the door" as well as good meaningful skills "record himself" and "play back the recording."

This is a much deeper mechanic, with a much less vague set of Activity Statements.

## Exercise to Improve Depth 练习提高深度

So you've got your design and it had what seems like a killer Activity Statement. But now you've prototyped it (or gone even further) and somehow the mechanic still ends up feeling flat. What can you do now?

First take stock:

1. Identify and list your objectives.

	a. For each, ask yourself: "Is this objective functionally a duplicate of any of the other objectives in my list?" If it is, ask yourself if you really need it. Do you really want to spend the time on teaching your players how to interact with it? If the answer is no, cross it out.

2. Identify and list all your meaningful skills.

	a. For each ask yourself: "Is this really a meaningful skill? Not too basic? Not an objective?"

	b. Ask yourself: "Is this skill functionally a duplicate of any of the other meaningful skills in my list?" If it is, cross it out. You're tricking yourself into thinking you have more skills than you actually do.

Having taken stock, do you now find you have too many objectives? Not enough meaningful skills? At this point, I'll bet you've discovered that, yes, somehow that's happened. At this point, just do the same exercise I suggested above to help my past-self get over his tractor beam problems:

1. Add one or more new meaningful skills to the list.

	a. As you add them, ask yourself the same questions as above. "Is this skill really meaningful? Is it too basic? Is it really an objective?"

2. Go through all your challenges and improve your Activity Statements

3. Prototype the new content.

4. Play-test. Is your problem solved? If so, then you're done!

5. If your problem isn't solved, go back to step 1 and try again.

## Something to Consider 需要考虑的事情

As I mentioned at the beginning of this article, making a mechanic deeper might not always be something you want to do. Before you go ahead and do a ton of work making a mechanic deeper, ask yourself these questions.

- If you make this deeper, will it still be appropriate for your audience?
	- Games intended for very young players tend to have a wide variety of fairly shallow game mechanics. The Lego Star Wars games have done this incredibly well.
	- Does your audience really want depth? Some more casual games might benefit from more activities rather than deeper ones (a fitness game, for example, or something like WarioWare).
- "Charming" game mechanics
	- If the main purpose of your mechanic is to express charm or personality, depth and increased challenge might actually get in the way of your players experiencing the quirkiness of your challenge.
	- A good example of this might be the deeper Clank gameplay I mentioned from Ratchet & Clank Future: A Crack in Time.
		- The action-recording gameplay received very mixed reviews.
		- Further, it required a ton of training and many help messages to teach players how to execute the new moves. For a mechanic that was a very small part of the whole game, this may not be ideal.
		- Players might look at the Clank sections as more of an amusing diversion. Needing to learn much deeper new skills might have alienated some.
- Narrative game mechanics
	- Sometimes the purpose of a game mechanic is to make the player "feel" something, or to serve some similar narrative purpose. In this case, depth and increased difficulty might actually get in the way of the player feeling what you want him to feel.
	- Indigo Prophecy and Heavy Rain are two games that made very good use of these kinds of Narrative Mechanics overall.

So once you've thought it through and decided that depth is what you want, you need to make sure your game mechanics each have clear objectives and a good set of meaningful skills. By using the exercises described above, and remembering the distinction between too-basic skills and meaningful skills, you'll have a better chance of evaluating, troubleshooting, and adding depth to your game mechanics.

## A Final Note 写在最后 
In this article I advocate a specific way of breaking down game mechanics. I pointed out the importance of focusing on Meaningful Skills over objectives and "too-basic" skills.

Though I believe this is a very useful way to break things down, I don't mean to imply that this way of breaking things down is somehow "more correct" than any other way. This is one of many very valid ways a game can be broken down.

In his book _Science and Method_ the French Mathemetician Henri Poincaré said "Geometry is not true, it is advantageous."

The same is true with models for thinking about game design. None of them are true -- they are only convenient. The model I suggest above has, time and again, proven quite convenient for me to use evaluating, predicting, and troubleshooting depth in game mechanics. Use it for that purpose, and it'll be your best friend. 
