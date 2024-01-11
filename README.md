# Pre-Year4 Unity Tools R&D
This isn't a university project; I did this work during the holidays between year3 and 4, it was self-directed and self-motivated. It's a few general purpose tools, that I would've used in all my Unity projects going foward; The tools aren't specialized to any kind of game, though they were informed and based on my work-flow, in Unity. A help-message inspector, a reference inspector, and a readme inspector. There're also some minor things scattered in there. I had plans for more tools too, you can maybe find details on those ideas in the docs.

With the Unity pricing debacle I stopped developing these; I no longer look to use Unity for my personal projects. The pricing didn't really matter to me, it was the fact that I saw Unity as a company that did not have my best interests in mind. To give a summary, Unity seems to be more of a platform than a game-engine; I also think, that if I kept using it, it'd limit my learning. I prefer the design of the other engines out there too, even if Unity is currently a faster workflow. It does suck, I was really exicted about making these tools, and I put in a lot of work, but it is what it is. 

## Reference Inspector (SceneAssetRefEditor)
The reference inspector is based on a reference-parameter typing system, that I had naturally come to create, after enough time with Unity. For value parameters, I typically used scriptable objects to store that data; Even for a one-off component like a player character controller, I'd still store their move-speed, jump-height, and so on, inside a scriptable object. Reference parameters are what was on my monobehaviours and stuff, their value data would be recieved via a scriptable object reference. For project management purposes, I found myself categorising them in one of three fields: Required reference, Nullable required, Component nullable. At first I did this through editor headers and stuff, eventually I wanted to automate that though, hence the reference inspector.

- Required Reference is a reference that the script cannot function without.
- Nullable Required is a reference that the script is supposed to have, and won't fully function if it doesn't have, but it won't crash the game or throw errors when it doesn't have the reference.
- Component Nullable is a reference that is completely optional as a parameter.

This wasn't going to be an editor, this was a literal inspector, like the one Unity uses to show you the editors of components. It was a seperate way to view data, a seperate unique inspector, that'd format data differently. The plan was to tag parameters using C# attributes, with their relevant category/type, and then this inspector would automatically show an editor, specifically for the references. It even worked with inherited paramters; An annoyance I always had was that if you inherited a type, you'd jumble up things like headers, that were created through drawer attributes; I'd end-up with multiple headers saying "Required References", it was messy. It'd essentially force you to create a custom editor for the component, which is time-consuming.

Unity didn't really support creating your own custom inspectors, so I had to bodge an inspector together with some of the lower-level GUI tools. Eventually I got it working exactly as Unity's does, I just thought I'd mention that.

It was also going to display warning messages in the HelpMessage inspector or something; I say "or something" because I think I eventually cancelled the HelpMessage inspector, becuase it was kind-of useless, or that it couldn't be implemented properly. But yeah it was going to use the HelpMessage GUI thing in Unity to display messages of varying warning levels, to inform that a component had, say, a RequiredReference missing.

## ReadMe Inspector (SummaryViewer)
The ReadMe Inspector was going to display text documentation of any inspected object. Again, it would function as a literal inspector, not an editor; I repeat this because people often call custom editors custom inspectors, it can be confusing. It was going to do this via there being a parralel scriptable object, storing the readme message, for every object. My plan was to automatically generate some of these by extracting the documentation comments in code. I would also have tools for myself to manually create them and override the generation functionality, incase I wanted to edit the message to be different from the auto-generated one. It would've been awesome, I could've even gitignored the generated ones. It could've been adapted to utallise the XML tags too, so that things like headers would show in the ReadMe inspector. I think this would've been super powerful, developers wouldn't have to exit unity or check the code to understand scripts that had been documented, they could work fully in Unity.

However, there were some issues that came up from this. Mainly with generating the scriptable objects from the documentation comments. The issue with this is C#, and that it is like having another compiler step. These things aren't deal-breakers but it'd mean that it'd be quite a difficult thing to create. To extract documentation comments, you have to read the script files themselves; I did this by utalising the code analysis package from microsoft. The problem with having to read the files though, is that class declerations are not linked to files; You can have multiple classes in one file, you can even have a single class spread across multiple files, through partial classes. Accounting for these cases would mean quite a bit of complexity in the code, it could be done, you'd just have to do a bit of bug-fixing. The performance impact of having to generate these SOs was unknown, my plan was to use a scriptable object that would act as a control panel, utallising references to assembly definitions, to manage the performance.

## HelpMessage Inspector
With multiple inspectors, I'd expect the dev to not always have each of them open. The purpose of the help message inspector was that it'd provide a central hub for each inspector to post their messages to. That way the dev can be notified of something they should do on an inspector that they don't currently have open. I think I eventually dropped this idea. I can't remember exactly why, it might've been that the implemtation wasn't working out, or that I planned to just use the default inspector for this. I think it might've been that I had come to think that help messages are too niche to need a central hub, I remember realising that there wasn't enough uses for them, or atleast with how you had to implement them.

