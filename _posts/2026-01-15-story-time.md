## Story Time

### Here comes Generative Artificial Intellegence

Our family is big on reading, learning, entertainment, filling time, we read a lot. Part of the bedtime routine in our house is reading. Some months ago my wife came across an app on the Apple App Store that would take inputs (a prompt!) and generate a bespoke story book. This was a great source of joy to our daughter, whatever silly idea was in her head could be manifested. Suddenly the nights I put her to bed were less fun, because I did not have access to this app (a white lie, I'm just more picky about what goes on my smartphone). Sometime later I discovered the Storybook app in Gemini, problem solved... right? For whatever reason the app my wife had been using was yanked from the app store, and her phone, the bedtime landscape shifted!

Maybe not in a good way, my wife doesn't have a Gemini Pro subscription, suddenly she can't make stories for bedtime with our daughter. I did enjoy being the favorite bedtime parent, but I'm also a problem solver, and there is a problem here. 

I had started dabbling in GenAI. I can make an app to generate bedtime stories!

#### Claude Code

Can I make this app? Yes, can I make it myself before my daughter is a legal adult given the amount of spare time I have? Probably not.

Enter my new junior dev, Claude. Claude is not great at keeping track of the big picture, but it can write code a lot faster than I can. The problem has now shifted from not having enough extra coding time to build this app, to learning how to manage a savant junior developer. 

After a few abortive attempts to get things working I took a beat to think about Claude’s capabilities, what it can access, how it keeps track of things, and I came upon the idea of putting a file in the repo for this code `specs/description.txt`. This file is where the application is described, it can be broad strokes, but the more specific the ask for any LLM the better the result. 

What I’m really doing here is building the scaffolding for the overall project. And I can have Claude start enhancing that scaffolding. So what is my first prompt for Claude?

```
Look at @specs/description.txt use the information there to write a full application spec, ask questions instead of deciding for yourself when there’s a decision to make regarding architecture, code language, or any other substantive decisions
```

Out of this we now have `specs/application-spec.md` which took Claude about 30 seconds to write, and would have been a few hour task for my slow meat brain. My job here now becomes reviewing the application spec and making sure it aligns with the description I provided. Validation of the approach of writing a description and then working with Claude occured here. There is a revision history at the bottom of application-spec.md there’s only 2, and the one revision was to add Comic Book formatted generation because I didn’t want my Son who is a few years older to be left out. 

As I sometimes have to ask Claude: What’s next? We need a development plan. Claude works a lot better if it has well defined and reasonably sized pieces of work that there is a clear objective on the other side of. Feeling brave, and like there is enough context with our two documents I decide I’m going to let Claude write its own development plan `specs/developmentplan.md`. Everything is looking good so far, one thing I noticed in the development plan document was “The project is structured in 5 major phases, from foundation to advanced features, with an estimated timeline of 16-20 weeks for MVP and 24-32 weeks for full feature set.” This is likely if a single human, or a small team of humans were developing this application. In reality it took 5ish days with just me and my dev partner Claude. Is it totally optimized? Probably not, is it production ready? No, Does it work as well as I need it to? Absolutely! And my daughter is still the same age as when I started!

I could write a whole post about where it’s smart and/or appropriate to use “vibe coding” but I’m going to go ahead and skip that for now.

Here is what Claude and I created:

### StorAI-Booker

![alt text](https://jshanna.github.io/images/storytime/storaibooker-01.jpg "StorAi-Booker landing page")

Here we see the landing page, it’s not too exciting, but lets go ahead and generate a story.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-02.jpg "Story generation template view")

Here we can select a template with examples filled in here’s a generation of “The Brave Little Dragon” template [Ember’s Brave Sky Journey](https://jshanna.github.io/artifacts/Ember-s-Brave-Sky-Journey.pdf). But let’s make something bespoke.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-03.jpg "Story topic")

We’ll make the target audience age 10, and why not a self-referential topic?

What we’re doing here is starting to build a prompt, each of these fields end up feeding into a Langchain flow that uses several agents to build a coherent storybook or comic.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-04.jpg "Story setting")

We’ll give a loose description of my home office in the Setting, leave the Format option on Storybook, and use the default 10 pages. The number of pages impacts the generation time as each page goes through several agents for artifact generation -> review -> acceptance/rejection -> regeneration -> repeat as needed, with a configurable limit on the number of times an artifact can be rejected and regenerated.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-05.jpg "Character settings")

We provide some characters, John is supposed to be me, Kate and Monty are not my kids names, but they can fill in for my kids. The descriptions are fairly vague but between the name, age, and description of hair styles we should get what we’re looking for.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-06.jpg "Predefined illustration styles")

There are a number of pre-defined styles, these don’t really have anything behind them (a potential improvement) they’re inserted into the prompting for image generation.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-07.jpg "Custom illustration style")

Because it is just a straight field -> prompt input we can easily inject a custom style. “Zany Anime” sounds fun to me, and is often what I ask Gemini to do to pictures of family and friends. 

![alt text](https://jshanna.github.io/images/storytime/storaibooker-08.jpg "Story generating...")

Once we click Generate Story we’re taken to the Library pane where we can see any stories that are being generated, and our already generated stories.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-09.jpg "Story dot menu")

One of the things I struggled with initially was getting the agent flow to generate consistent characters. There’s a story building flow that works in a pretty typical outline -> expand -> review -> refine process, but not so much for the illustrations. Initially there was nothing to help character consistency across pages except what was in the prompt for that illustration. I was talking to a friend who develops media generation/editing agent flows for his day job and he suggested injecting a step that creates a character sheet for each character, and voila consistent illustrations!

![alt text](https://jshanna.github.io/images/storytime/storaibooker-10.jpg "Character sheets")

Clicking on View Artifacts we can access those character sheets, the characters in the generated storybook should be consistent with what we see here… why are “my” arms all splotchy? Maybe there’s a narrative reason for that?

![alt text](https://jshanna.github.io/images/storytime/storaibooker-11.jpg "Page text and image generation prompts")

And here we can see the page illustration prompts with (SPOILERS!) the text for each page. I had to do some log diving to get this information while struggling with character consistency, and I thought it would be interesting to make these prompts accessible so I could try them out in other image generation systems.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-12.jpg "Generated story cover page")

When we click on Read Story a reader view opens, and we can read the story. On the cover we can see the character prompts worked pretty well, while not exactly what my kids look like they would probably recognize themselves especially if I used their real names in the text.

![alt text](https://jshanna.github.io/images/storytime/storaibooker-13.jpg "Generated story page")

Here we see page 2, I can’t say I’ve ever sat in an office chair like that, but otherwise it looks pretty good. I’m not going to screenshot every page, you can find a PDF export of this story here: [Dad's Digital Story Dreams](https://jshanna.github.io/artifacts/Dads-Digital-Story-Dreams.pdf). Speaking of…

![alt text](https://jshanna.github.io/images/storytime/storaibooker-14.jpg "Generated story export options")

What if you want to take a bunch of the content you’ve generated off-grid. You can export your stories to several formats and load them on a tablet or other device and take them with you.

THE END

So what did I learn through this project? A lot! Primarily I realized that the way I interact with an LLM while working on projects needs to be well considered and clear. The more complete the context I can provide the more likely I am to be pleased with the output from the LLM. 

Is this a finished project? Not really, but it’s at a good place for my family to be able to start generating stories. Of course my daughter has moved on to a book series about an owl that hates squirrels so she hasn’t been asking to make her own stories lately, but we’re future proofed. There’s a ton of stuff I didn’t cover like content restrictions, sharing (multiple users are functioning, and you can share stories into a “public” library), and a few other settings.

Thanks for reading, and please don’t hesitate to let me know what you think!

[Project Repository](https://github.com/jshanna/storai-booker)