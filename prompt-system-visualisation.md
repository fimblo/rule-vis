Hi there.

Here's a painpoint I'd like to address: Organisations create rules, policies, guidelines and processes to as a framework to support the work which in turn creates outputs and hopefully create impact. (Going forward, I'll just use the word "rule" to refer to policies, guidelines, processes, etc)

An org with no rules is dysfunctional. Titles and roles are a type of rule ("Engineering managers are accountable for engineering decisions"). Target-setting frameworks are also rules ("Our planning cycles are done quarterly", or "We use Cascading OKRs as our planning and communication framework"). So rules are necessary and can not only be good, but are essential for a group of people to operate in tandem.

But over time, these rules grow in number, making it increasingly difficult to find time/energy to do the actual work which can lead to impact.

At some point, if the number of rules continue to grow in number and complexity, they find that the rules constrain them so much that they need to remove or change them.

But removing existing rules is very hard - since it's a form of the tragedy of the commons. It's easy for a member of the group to add a rule which they believe will benefit the group, but it's really difficult for another member to remove said rule.

What I'd like to do is to translate one or more rules in natural language into something formal. Then take the resulting instructions and feed it into a tool which visualises these rules superimposed in one visualisation.

By visualising the rule set, we might be able to draw interesting conclusions and in turn it might give rise to powerful questions.

--

# Here's some more details with regard to the rule translator:


In my mind, a rule has these components:

- The rule name
- The entity creating it.
- The entities for whom it's for
- The type of rule
- The utility for the person creating it
- the utility for the persons who need to follow it

## The rule name

Exists primarily as a handle to enable us to talk about a specific rule instance. 

## Entities

In the simple case, entities are people. But it could very well be a group of people, or a group of groups. It could also be "the customer" or "the climate" or "the coffee machine" if the rule in question is about these. Or an entity could be another rule, referred to by name.

An entity will have a name and a type:

```
(
  name,
  type
)
```

For now while discussing this let's stick to people for simplicity's sake. So if we want to refer to Bob, we have:

```
(
  "Bob",
  "person"
)
```


## Types of rules

- Positive rules: Person A has the option to do X.
- Negative rules: Person A may not do Y.
- Atomic Procedural rules: If Person A wants to act on X, they must/may not do it using recipe Z.
- Complex rules: A rule which combines positive, negative, atomic procedural and even other complex rules.

There's probably more, and the ones I wrote might need modification. But take this as something we can work with going forward. Eventually I think we might need go full Backus-Naur and create a proper formal notation, but for now (in ideation phase) I think the above is sufficient.

## Utility

Though rules tend to be birthed for "a good reason", they are rigid while the org is fluid and changes over time. A "good rule" can in another temporal context be a bad one. This introduces lots of complexity into the model, so instead of going deep here, I'd rather we think in form of the utility for the creator of the rule and the target(s) of said rule.

At this point, I think we should stay away from measuring utility in well-defined units (like time). Instead we keep it simple by using these figures as relative weights.

If I create a rule which says "you pay me 100USD every day", then it's utility value for me is positive, and its value for you is negative. Yes, we'll assign a number, but for now it suffices with the values (1, -1).

Generally speaking, I think that positive and negative rules are cheaper for the target entity than procedural rules.


## Putting it all together

Done right, a rule might be translated into something like this:

```
( 
  rule_name,
  rule_type,
  source_entity,
  target_entity,
  source_utility,
  target_utility
)
```

A rule which targets five people would in this model create five rules (one for each person targetted). We can optimise this later.


## Translation

Given that the language model in (say) Backus-Naur form is complete, we would then be able to feed it, and a set of rules in human-readable form to an LLM to receive a list of formal structured rules as a list.

Please note that we don't need to represent what the rule "is" in the model at this stage. All we need is to represent the utility and who benefits and who doesn't. Anything else will probably open a huge layered can of worms :)


# Visualisation

The next step is to create a tool which, given a list of rules in structured format, can visualise it.

The visualiser will probably add some data to be able to plot the rules. For example, the entity object could be extended with a `point(x,y)`, so that it can be plotted in a 2D graph.

The rule can then be used to draw an edge between two entities. The utility can be visualised in some way (let frothing packs of UX designers run wild)

I'm thinking that the visualiser will be the tool which we spend most time with. 

Initially it will probably be a read-only tool which only lets the user rotate, zoom in, etc. Some kind of toggle which enables or disables utility summation could be cool (a rule between A and B's utilities get cancelled out if there is another rule between B and A with a reverse utility).


But later I'm curious what would happen if one could change the utility balance for rules. or include a small "tax" which is added for every rule targetting an entity (to emulate context-switching cost, frustration, or the sense of lack of agency etc). But that's for the future.

# Questions to you

I want you to think first as a Product person, expert at slicing work. Then, I'd like you to think as an software engineer with experience working with formal programming language theory (grammars, parsers, maybe even lambda calculus).

My explanation above is rather waterfall. Do language, then do visualisation. In my experience, waterfall is rather risky, since by the time the work is done, the problem might have changed. 

But in this age of super-quick development times, I don't think it matters _that_ much. But am curious what you think.


So given the text above as inspiration and not gospel, how would you slice the work? 


