# When the Model Can't Be Fixed: Protecting Play in a Computer Vision Pipeline

A child points a wand at a picture book. A dinosaur roars. Magic.

That magic runs on computer vision. At Kibeam Learning, I worked on screenless interactive audio experiences where ML models detect words and objects in physical books, triggering narration, sound effects, games, and gesture-based interactions with a handheld wand.

<!-- Visual of Kibeam Wand + Computer Vision + Physical Book + Responses -->

When inference works, it's invisible. When it breaks, a child (and their parent!) loses trust in the experience. They pointed at something. Nothing happened. Or worse, the wrong thing happened, and the story betrayed them.

My job was to make sure that didn't happen.

I operated at the seam between Creative and Engineering. I translated scripts into the XML and audio sequences that run on the wand, and diagnosed failures when the system didn't behave as designed. Some fixes took minutes. Others took weeks and required me to learn when to stop chasing technical solutions and protect the experience through design instead.

<!-- Visual of How Do Dinosaurs Learn to Read Book Cover -->

This case study walks through three inference issues from a single book: How Do Dinosaurs Learn to Read? Each required a different solution. Together, they show how I diagnose problems across the full stack, and when I learned to pivot.

---

## Ticket #DINO-80 ~ Know Where to Look

A child points at the tiny book clutched in the dinosaur's claws on the title page. They expect a roar. They get silence.

<!-- Visual of title page with r.book circled and a note saying will not infer -->

QA flagged an inference issue. The model wasn't detecting the r.book target. Earlier in my time at Kibeam, my first instinct would have been to dive into the logs and verify whether the computer vision model could see the object at all.
But experience taught me to check the script first.

<!-- Screenshot of missing entry for r.book in script -->

Sure enough, the entry for r.book was missing a hover component, the instruction that tells the system what to do when the model detects a target. The model was seeing just fine. The system just didn't know how to respond.

I added the component, created a new build, tested locally, and…

"ROAAAAAAR."

---

## Ticket #DINO-84 ~ Acceptable Trade-offs

A child is exploring page 21, pointing at words, hearing them read aloud. Suddenly, without warning, the wand thinks they're on page 18. The child did nothing wrong. The book just stopped making sense.

QA flagged "ejections" occurring when pointing at the text on page 21. I narrowed the problem area to surrounding the phrase "She reads," which appears on pages 21 and 18. The model couldn't tell those spots apart.

<!-- Screenshot of labeled words and a note saying ejects to page 18! -->

I started with the least invasive fix, adjusting the FOV (field of view). This parameter controls how much of the page the model sees. More context should help it distinguish between pages. I bumped up the value in small increments. No improvement. At FOV 37, I started seeing false triggers elsewhere. Ugh, new problems in exchange for no solution.

Next tool: relabeling and retraining. I added specific labels to each text region and retrained the model. This approach had worked on a different book. But on this one, it didn't. The model still ejected the user from page 21 to 18.

I escalated to our ML engineer. He suggested trying a different training recipe. Still no fix.

At this point, I'd exhausted technical options that would give us a "perfect" fix. So I shifted to negotiation.

I proposed an exclusion rule to our producer. This meant that when the user is on page 21, the system ignores page 18's text targets entirely. No more ejections, but there's a trade-off. Users can't navigate directly from page 21 to page 18 by pointing at page 18's text. However, they could point at any other element on the page.

We agreed that a minor navigation constraint was better than a disorienting failure. I'd tried everything else first. That made it easy to say yes.

The fix shipped.

---

## Ticket #DINO-101 ~ The Wall

Holding the wand, a child's hand drifts juuust off the edge of page 21. By design, nothing should happen. Instead, the wand launches into a mini-game. "Hey, what's going on?!" the kid exclaims. Suddenly, they're somewhere else, smack in the middle of an interaction that they didn't ask for.

That's not magic! That's betrayal.

<!-- Screenshot of area off book circled and note saying launched dino_head game! -->

QA filed a tricky inference ticket days before our ship date. Pointing off-book near the bottom of page 21 triggered the model to detect the dinosaur head on page 20... way on the opposite side of the book. I ran through my complete diagnostic toolkit: FOV adjustments, confidence thresholds, relabeling, and retraining. Nada. Nothing worked.

I escalated to our ML engineer. He attempted additional training, came back, and shook his head, "Unfortunately, it behaves about the same."

Our model was supremely confident, but supremely wrong.

With our deadline approaching, I brought the problem to my Creative Director. He offered a reframe I hadn't considered: "What if we move the game trigger off the dino head completely? Assign it to the book in the dino's claws instead. Then we put a short squawk sound effect on the dino head."

<!-- Screenshot of rearrangement of responses -->

I immediately clocked the elegance. If the model misfires and detects the dino head when a child points off-book, they hear a goofy "squawk." Unexpected, but harmless versus a catastrophic two-minute game they didn't ask for.

Confirming we'd launched numerous games off the dinos' books throughout the title, I built the change that afternoon. Tested it. Smiled.

We shipped, and the book went live on time. Still a model failure. But now, graceful.

---

> "When I filed computer vision tickets, Choksi would come back with a full breakdown of what he'd tried before proposing a workaround. I always knew he'd exhausted all options, and he communicated every adjustment throughout the process."
>
> — Alyssa, QA Engineer
