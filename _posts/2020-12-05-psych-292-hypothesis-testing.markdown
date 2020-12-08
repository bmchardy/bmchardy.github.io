---
layout: post
title:  "PSYCH 292: Hypothesis Testing"
date:   2020-12-05 11:00:00 -0400
categories: statistics tutorial
---
<div class="container-fluid main-container">

<div class="row-fluid">

<div class="toc-content col-xs-12 col-sm-8 col-md-9">

<div class="fluid-row" id="header">

# PSYCH 292: Hypothesis Testing

#### Bobby McHardy, for the University of Waterloo’s Psychology Society

#### December 5, 2020

</div>

In this PSYCH 292 tutorial, we are going to focus on t-tests. We’ll introduce the essential concepts as we walk through a pratical example.

While most of the focus of this tutorial will be on statisics, we’ll also look at how we can perform these calculations in a statistical programming language called “R”. I’ll put the “**Using R**” heading throughout the tutorial to denote optional information about the “R” programming language! Enjoy :)

<div id="our-data" class="section level1">

# <span class="header-section-number">1</span> Our Data

We’ll be conducting our t-tests on a dataset of N = 18 participants. These (example) data describe the experiences of our 18 participants in an experiment about Music Genres and Well-Being.

<div id="experimental-procedure" class="section level2">

## <span class="header-section-number">1.1</span> Experimental Procedure

We randomly asked 18 students in our PSYCH 292 class to participate. Each participant in the _experiment_ began by reporting their _baseline_ (initial) Well-Being score. Well-Being was _operationalized_ as a _self-report_ measure of their perceived health and happiness. Participants rated their Well-Being on a score from 1 (A very low Well-Being) to 7 (A very high Well-Being). Participants were also asked to report the number of coffees that they drink on an average day because we think that Coffee Consumption might be a _predictor_ variable of one’s Well-Being. Knowing how much coffee each participant drinks per day allows us to _control for_ Coffee Consumption.

After we measured our _baseline_ Well-Being score and asked about Coffee Consumption, we began the _experimental manipulation_. The _indepenent (manipulated) variable_ is Music Genre. We _randomly assigned_ each participant to one of three Music Genre _conditions_ (either Classical, Blues, or Control). In the Classical Music Genre condition, the participants were asked to listen to 15 minutes of Classical music when they first wake up for one week. Likewise, in the Blues Music Genre condition, the participants were asked to listen to 15 minutes of Blues music when they first wake up for one week. In the _control condition_, participants were not asked to listen to any music for the week.

At the end of the week, we measured (in a _post-test_) the Well-Being of each participant again using the same 1-7 _self-report_ measure.

**Recall** that this is an _experiment_ because we are _manipulating_ our _independent variable_ (Music Genre) and _randomly assigning_ participants to _conditions_ (_levels_ of the independent variable). To make sure you are clear on all of these terms, review your PSYCH 291 notes.

</div>

<div id="iv-dv-conditions" class="section level2">

## <span class="header-section-number">1.2</span> IV, DV, Conditions

In this dataset, the _independent variable (IV)_ is Music Genre (either Classical, Blues, or Control _conditions_). The _dependent variable_ is self-report Well-Being (from 1-7). We are also _controlling for_ Coffee Consumption.

</div>

<div id="dataset" class="section level2">

## <span class="header-section-number">1.3</span> Dataset

**Using R:** To load a datafile using R, we can use the following code.

    # In R, or other programming languages, we can assign values
    # to variables just like you would in research.
    #
    # In this case, we are assigning the contents of the datafile
    # on my computer to a variable called "wellbeing_intervention_data"
    # using the "<-" (assignment) operator.
    wellbeing_intervention_data <- read.csv("wellbeing_intervention_data.csv")

At the end of the week, these are the data we collected.

**Using R:** To display a table of data, simply type the name of the variable like so!

    wellbeing_intervention_data

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["Participant"],"name":[1],"type":["int"],"align":["right"]},{"label":["Coffees"],"name":[2],"type":["int"],"align":["right"]},{"label":["Music"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Baseline_WB"],"name":[4],"type":["int"],"align":["right"]},{"label":["Post_WB"],"name":[5],"type":["int"],"align":["right"]}],"data":[{"1":"1","2":"3","3":"Control","4":"4","5":"5"},{"1":"2","2":"3","3":"Blues","4":"5","5":"3"},{"1":"3","2":"3","3":"Classical","4":"4","5":"4"},{"1":"4","2":"1","3":"Control","4":"2","5":"3"},{"1":"5","2":"4","3":"Classical","4":"5","5":"5"},{"1":"6","2":"3","3":"Control","4":"4","5":"5"},{"1":"7","2":"0","3":"Blues","4":"6","5":"2"},{"1":"8","2":"3","3":"Classical","4":"4","5":"5"},{"1":"9","2":"4","3":"Control","4":"4","5":"4"},{"1":"10","2":"2","3":"Control","4":"4","5":"4"},{"1":"11","2":"2","3":"Blues","4":"4","5":"1"},{"1":"12","2":"3","3":"Classical","4":"5","5":"6"},{"1":"13","2":"1","3":"Blues","4":"5","5":"2"},{"1":"14","2":"3","3":"Classical","4":"5","5":"6"},{"1":"15","2":"4","3":"Blues","4":"4","5":"4"},{"1":"16","2":"0","3":"Control","4":"5","5":"4"},{"1":"17","2":"3","3":"Classical","4":"4","5":"7"},{"1":"18","2":"1","3":"Blues","4":"4","5":"1"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

The columns of this data table are _Participant_, _Coffess_ (the number of cups that each participant reporting consuming daily), _Music_ (the condition; one of _Classical_, _Blues_, or _Control_), _Baseline_WB_ (the level of Well-Being each participant reported at the beginning of the experiment; between 1 and 7), and _Post_WB_ (the level of Well-Being reported at the end of the week-long intervention).

</div>

</div>

<div id="questions" class="section level1">

# <span class="header-section-number">2</span> Questions

So what are some _research questions_ that we might want to answer?

1.  **“Does Blues music _cause_ university students to experience different levels of well-being than Classical music?”**
2.  **“Does one week of classical music _lead to_ greater reported well-being?”**

**Note** that _lead to_ is another way to make a causal claim.

**Recall** that we can make a _causal claim_ because our data are _experimental_ (and abides by the 3 rules for causal claims). If you’re unsure why this means that we can make a causal claim, look back on your PSYCH 291 notes.

How do we answer these questions?

</div>

<div id="hypothesis-testing" class="section level1">

# <span class="header-section-number">3</span> Hypothesis Testing

Put simply, **hypothesis testing** is a way to test whether a _hypothesis_ we make about a _population_ is true or false.

The only way to prove _for certain_ that different music genres lead to different levels of well-being in the population of university students would be to experiment with _every_ university student in the world (the entire population). It’s not exactly reasonable for us to do that, so we have to settle for a best guess about what the sample data are telling us.

Doing this requires a new type of statistic (and a very powerful one). Before, we had to settle for descriptions of our sample (What is the mean? The median? The standard deviation?) These were called **descriptive statistics** because they described the sample of data exactly. For example, in our dataset above, the average number of coffees that the participants reportedly drink per day is exactly (rounded) `M = 2.389` and the number of participants in the experiment are `N = 18`. These statistics exactly describe aspects of our sample.

**Using R:** We can use R, for example, to find the well-being mean (descriptive statistic) for participants before and after a treatment of classical music.

    # subset selects only the rows of our wellbeing_intervention_data table
    # that meet the criteria (Music must equal "Classical"). We then make a
    # new variable called "classical_participants" that contains only that data.
    classical_participants <- subset(wellbeing_intervention_data, Music == "Classical")

    # Next, we can calculate means of the Baseline_WB and Post_WB columns
    # (and assign those means to numerical variables)!
    classical_baseline_wellbeing_M <- mean(classical_participants$Baseline_WB)
    classical_post_wellbeing_M <- mean(classical_participants$Post_WB)

    # We can even "print" (display) the results for all to see like so:
    print(paste0("Before the intervention, the classical condition reported an average well-being of ", classical_baseline_wellbeing_M, ". After the intervention, these same participants reported an average well-being of ", classical_post_wellbeing_M, "."))

    ## [1] "Before the intervention, the classical condition reported an average well-being of 4.5\. After the intervention, these same participants reported an average well-being of 5.5."

This shows us that well-being is higher on average in our sample after the weeklong intervention for the Classical group. But does this mean we have enough information to confirm our second research question (“Does one week of classical music _lead to_ greater reported well-being?”) No.

In the case of _hypothesis testing_, we cannot look at our sample and say for certain that “yes, one week of listening to classical music leads to greater well-being in the _population_” because there is no way to know this about the population for certain just based on our sample; any sample has _sampling error_. We’re also moving away from descriptive statistics with this research question because we are no longer looking to _describe_ our sample, we are looking to _infer_ conclusions from the sample about the population.

We need to test our hypotheses using **inferential statistics**: _inferences_ we make about the _population_ using our sample. These statistics are no longer ‘facts’! These are guesses about what our sample is saying based on probability.

</div>

<div id="the-hypothesis" class="section level1">

# <span class="header-section-number">4</span> The Hypothesis

When we are testing our hypothesis, we talk about the _null hypothesis_ and the _alternative hypothesis_.

The **null hypothesis**, written <span class="math inline">\(H_0\)</span>, is the hypothesis that this difference does not exist. We might say that one group is _equivalent_ to the other group. In our example, the _null hypothesis_ is that “there is no population difference in well-being between baseline and after the Classical music intervention.”

The **alternative hypothesis**, written <span class="math inline">\(H_1\)</span>, is the hypothesis that there is a difference. We might say that one group is _not equivalent_ to the other group. In our example, the _alternative hypothesis_ is that “there is a population difference in well-being between baseline and after the Classical music intervention.”

</div>

<div id="the-t-statistic" class="section level1">

# <span class="header-section-number">5</span> The _t_ Statistic

**Question:** How do we know if there is a difference between two groups?

You might be wondering if we can just simply compare the sample means of both groups and see if they’re the same or not. This doesn’t really work.

For our example, <span class="math inline">\(H_0\)</span> = “no population difference in well-being between baseline and post-classical music intervention.” <span class="math inline">\(H_1\)</span> = “there is a population difference in well-being between baseline and post-lassical music intervention.”

We know that the average well-being of the sample before the Classical music intervention was 4.5 (the ‘pre-test’ group). We also know that after a week of classical music the mean was found to be 5.5 (the ‘post-test’ group) in the sample. We can see that the two are different by 1\. So we _know for a fact_ that the means in the _sample_ are different.

But that’s not what we’re asking. We’re asking if these means are different in the _population_. This is a trickier question because we did not survey the entire population of students (of course).

<div id="remember-z-score" class="section level2">

## <span class="header-section-number">5.1</span> Remember _z_-Score?

Remember the _z_-Score from earlier in the course?

<span class="math display">\[z = \frac{x-\mu}{\sigma}\]</span>

<span class="math inline">\(z\)</span> is a great way to figure out how ‘far’ from the population mean (<span class="math inline">\(\mu\)</span>) a value is, in terms of standard deviations of the population (<span class="math inline">\(\sigma\)</span>). For example, if we knew that the (all-time) average December morning in Waterloo is -3°C (population <span class="math inline">\(\mu = -3\)</span>) with a standard deviation of 1°C (<span class="math inline">\(\sigma = 1\)</span>) and we measured that the morning of December 4th was -8°C (<span class="math inline">\(x = -8\)</span>) in Waterloo, we could calculate a _z_-Score as follows:

<span class="math display">\[z = \frac{x-\mu}{\sigma} = \frac{(-8)-(-3)}{1} =-5\]</span>

This _z_-Score tells us that the morning of December 4th is actually 5 standard deviations below the average population temperature! Remember that we could look at our _z_ table and see that only less than 0.00004% of days are expected to be below this level, so this is significantly below average! Brrrr

We can also use _z_-Scores to compare different samples when we’re given the population standard deviation. If a sample of 30 people before the classical music intervention had an average well-being score of 2 (<span class="math inline">\(\mu_1 = 2\)</span>) and if the same people after the classical music intervention had an average well-being score of 4 (<span class="math inline">\(\mu_1 = 4\)</span>), we could calculate the _z_-Score. Of course, we need to know the standard deviation. If we are given the population standard deviations, then we’re good to go. In this case, let’s say that <span class="math inline">\(\sigma_1 = 1.5\)</span> and <span class="math inline">\(\sigma_2 = 1\)</span>.

We need the standard error (<span class="math inline">\(SE\)</span>), as you’ve done before, as follows:

<span class="math display">\[SE = \sqrt{\frac{\sigma_1^2}{n} + \frac{\sigma_2^2}{n}} = \sqrt{\frac{1.5^2}{30} + \frac{1^2}{30}} = 0.32914\]</span>

<span class="math display">\[z = \frac{\mu_2-\mu_1}{SE} = \frac{4-2}{0.32914} = 6.08\]</span>

We end up with a _z_-Score = 6.08\. Remember that we’re testing our _null hypothesis_ that the two groups are the same (no difference). If there were no difference between the groups, we would expect there to be pretty close to a zero standard deviation difference between them, but we found a difference of over 6 standard devations!

If we search up the _z_-value in a _z_ table, we see that this _z_-value is above the 99.999% of all possible z-values..

    tbl_z

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["Z"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["percent"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"0.0","2":"0.5199400"},{"1":"0.1","2":"0.5596200"},{"1":"0.2","2":"0.5987100"},{"1":"0.3","2":"0.6368300"},{"1":"0.4","2":"0.6736400"},{"1":"0.5","2":"0.7088400"},{"1":"0.6","2":"0.7421500"},{"1":"0.7","2":"0.7733700"},{"1":"0.8","2":"0.8023400"},{"1":"0.9","2":"0.8289400"},{"1":"1.0","2":"0.8531400"},{"1":"1.1","2":"0.8749300"},{"1":"1.2","2":"0.8943500"},{"1":"1.3","2":"0.9114900"},{"1":"1.4","2":"0.9264700"},{"1":"1.5","2":"0.9394300"},{"1":"1.6","2":"0.9505300"},{"1":"1.7","2":"0.9599400"},{"1":"1.8","2":"0.9678400"},{"1":"1.9","2":"0.9744100"},{"1":"2.0","2":"0.9798200"},{"1":"2.1","2":"0.9842200"},{"1":"2.2","2":"0.9877800"},{"1":"2.3","2":"0.9906100"},{"1":"2.4","2":"0.9928600"},{"1":"2.5","2":"0.9946100"},{"1":"2.6","2":"0.9959800"},{"1":"2.7","2":"0.9970200"},{"1":"2.8","2":"0.9978100"},{"1":"2.9","2":"0.9984100"},{"1":"3.0","2":"0.9988600"},{"1":"3.1","2":"0.9991800"},{"1":"3.2","2":"0.9994200"},{"1":"3.3","2":"0.9996000"},{"1":"3.4","2":"0.9997200"},{"1":"3.5","2":"0.9998100"},{"1":"3.6","2":"0.9998700"},{"1":"3.7","2":"0.9999100"},{"1":"3.8","2":"0.9999400"},{"1":"3.9","2":"0.9999600"},{"1":"4.0","2":"0.9999700"},{"1":"4.1","2":"0.9999800"},{"1":"4.2","2":"0.9999900"},{"1":"4.3","2":"0.9999900"},{"1":"4.4","2":"1.0000000"},{"1":"4.5","2":"1.0000000"},{"1":"4.6","2":"1.0000000"},{"1":"4.7","2":"1.0000000"},{"1":"4.8","2":"1.0000000"},{"1":"4.9","2":"0.9999996"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

It’s a crazy outlier. Remember that we only need to be above the 97.5% mark (<span class="math inline">\(p < .05\)</span>) to reject the null hypothesis. We’re well past it, so we reject <span class="math inline">\(H_0\)</span>; we say “there is a significant difference between the two groups - our classical music intervention worked!”

So _z_-Scores are great, but we’ve just found a limitation to _z_-Scores here..

</div>

<div id="whats-wrong-with-the-z-score" class="section level2">

## <span class="header-section-number">5.2</span> What’s wrong with the _z_-Score?

Nothing’s _wrong_ with the _z_-Score, but it has its limits. **The _z_-Score can only be used if we know the _population_ standard deviation (<span class="math inline">\(\sigma\)</span>).**

In the example above, we had to be told that the first group had a population standard deviation of <span class="math inline">\(\sigma_1 = 1.5\)</span> and that the second group had a population standard deviation of <span class="math inline">\(\sigma_2 = 1\)</span>.

In the context of our problem: If I were to ask you what the average number of coffees a PSYCH 292 student drinks per day, what would you say? How would you measure it?

Being the astute PSYCH student that you are, you would probably gather a sample of PSYCH 292 students, ask them about their coffee drinking habits, find the mean, standard deviation and be on your way!

This is the right way to calculate the… sample… standard deviation, but it’s not the same as the population standard deviation.

In order to calculate the population standard deviation, we would need to find the average number of coffees that _all_ PSYCH 292 students at UW - and in the world, for that matter - drink. This is not at all realistic.

</div>

<div id="the-importance-of-the-t-statistic" class="section level2">

## <span class="header-section-number">5.3</span> The importance of the _t_-Statistic

This is where the _t_-Statistic comes into play! The _z_-Score lets us decide how different two samples are when we know the standard deviations of the populations from which the samples are drawn. As discussed above with the coffees, this is not always very practical.

**Usually we don’t know anything about the population.** In these cases, instead of a _z_-Score we use a _t_-Statistic.

The major difference between the two statistics is that **<span class="math inline">\(z\)</span>-Scores require us to know population <span class="math inline">\(\sigma\)</span>**, whereas **<span class="math inline">\(t\)</span>-Statistics only require us to know the sample <span class="math inline">\(sd\)</span>)**. We use these sample statistics to _estimate_ the population within a margin of error, our <span class="math inline">\(p\)</span>-value.

There are a few different types of _t_-Statistics, but they all involve calculating the difference between two values over (divided by) some measure of spread, just like a _z_-Score does.

</div>

</div>

<div id="the-two-cases" class="section level1">

# <span class="header-section-number">6</span> The Two Cases

There are two main cases where a _t_-statistic can be used.

<div id="dependent" class="section level2">

## <span class="header-section-number">6.1</span> Dependent

1.  Dependent _t_-Test (sometimes called a ‘paired _t_-Test’)

This is the one that is most similar to what we were just doing above with the pre- and post-test classical music groups, only now we do not need to be given population standard deviations, because _t_-Tests kindly let us use the sample sd’s!

It’s called dependent because we’re looking at differences within the same group of people, just over time or over some intervention.

Just as a _z_-Test looks like

<span class="math display">\[z = \frac{\mu_2-\mu_1}{\sqrt{\frac{\sigma_1^2}{n} + \frac{\sigma_2^2}{n}}}\]</span>

a dependent _t_-Test looks like

<span class="math display">\[t = \frac{M_2-M_1}{\sqrt{\frac{SD_1^2}{n} + \frac{SD_2^2}{n}}}\]</span>

It’s honestly the same thing, we just do something different in the table when we look up our value of significance. To repeat: a t-test is still calculating the number of standard deviations between scores.

Let’s say we have the same sample as our last example. The 30 people before the classical music intervention had an average well-being score of 2 (<span class="math inline">\(M_1 = 2\)</span>) and had an average well-being score of 4 (<span class="math inline">\(M_2 = 4\)</span>) after the intervention, just like above. The only difference this time is that we were **not** given the the population standard deviations. But we’ll assume that we had all the raw data, so we were able to find the standard deviations of each sample. Let’s say that <span class="math inline">\(SD_1 = 1.5\)</span> and <span class="math inline">\(SD_2 = 1\)</span>. These are the exact same numbers as above!

So, not surprisingly, our result is exactly the same - it’s just called a _t_-value instead of a _z_-value because we used sample standard deviations (estimates).

<span class="math display">\[t = \frac{M_2-M_1}{\sqrt{\frac{SD_1^2}{n} + \frac{SD_2^2}{n}}} = \frac{4-2}{0.32914} = 6.08\]</span>

So what’s this big difference? Well.. when we calculate sample standard deviations, we’re taking a bit of a “guess” because a sample will obviously not give us the exact standard deviation of the distribution. So really, using a _t_ statistic introduces a bit more error than the _z_ does because we’re estimating a bit more of the problem.

The benefit of the _t_-statistic is that it accounts for this extra risk with something called “degrees of freedom”.

**degrees of freedom** is a simple way to account for the added error induced by using estimated values in place of a true value. In this case, we’re using estimated (sample) standard deviations in place of true (population) standard deviations, so we need to take the problem’s degrees of freedom into account.

The biggest issue with using an estimate is that we’re only using a sample number of people, not every person in the entire universe (like the population does). By having a small <span class="math inline">\(N\)</span> like this, we take on a greater risk that the sample <span class="math inline">\(SD\)</span> is not 100% accurate. In fact, the smaller the <span class="math inline">\(N\)</span>, the greater chance of error there is.

For example, it only makes sense that if I take the well-being scores of <span class="math inline">\(N = 1000\)</span> PSYCH 292 students, I’ll have a better idea of what the mean and standard deviation of the population is than if I only look at the well-being of <span class="math inline">\(N = 5\)</span> PSYCH 292 students.

Degrees of freedom take this into account because degrees of freedom is actually defined as follows:

<span class="math display">\[df = N - 1\]</span>

When degrees of freedom is a direct function the number of people in the sample (<span class="math inline">\(N\)</span>) like this, we are able to use degrees of freedom in our statistical decision making process.

Let’s introduce the _t_ table:

    tbl_t

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["df"],"name":[1],"type":["int"],"align":["right"]},{"label":["significance_value"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"6.313752"},{"1":"2","2":"2.919986"},{"1":"3","2":"2.353363"},{"1":"4","2":"2.131847"},{"1":"5","2":"2.015048"},{"1":"6","2":"1.943180"},{"1":"7","2":"1.894579"},{"1":"8","2":"1.859548"},{"1":"9","2":"1.833113"},{"1":"10","2":"1.812461"},{"1":"11","2":"1.795885"},{"1":"12","2":"1.782288"},{"1":"13","2":"1.770933"},{"1":"14","2":"1.761310"},{"1":"15","2":"1.753050"},{"1":"16","2":"1.745884"},{"1":"17","2":"1.739607"},{"1":"18","2":"1.734064"},{"1":"19","2":"1.729133"},{"1":"20","2":"1.724718"},{"1":"21","2":"1.720743"},{"1":"22","2":"1.717144"},{"1":"23","2":"1.713872"},{"1":"24","2":"1.710882"},{"1":"25","2":"1.708141"},{"1":"26","2":"1.705618"},{"1":"27","2":"1.703288"},{"1":"28","2":"1.701131"},{"1":"29","2":"1.699127"},{"1":"30","2":"1.697261"},{"1":"31","2":"1.695519"},{"1":"32","2":"1.693889"},{"1":"33","2":"1.692360"},{"1":"34","2":"1.690924"},{"1":"35","2":"1.689572"},{"1":"36","2":"1.688298"},{"1":"37","2":"1.687094"},{"1":"38","2":"1.685954"},{"1":"39","2":"1.684875"},{"1":"40","2":"1.683851"},{"1":"41","2":"1.682878"},{"1":"42","2":"1.681952"},{"1":"43","2":"1.681071"},{"1":"44","2":"1.680230"},{"1":"45","2":"1.679427"},{"1":"46","2":"1.678660"},{"1":"47","2":"1.677927"},{"1":"48","2":"1.677224"},{"1":"49","2":"1.676551"},{"1":"50","2":"1.675905"},{"1":"51","2":"1.675285"},{"1":"52","2":"1.674689"},{"1":"53","2":"1.674116"},{"1":"54","2":"1.673565"},{"1":"55","2":"1.673034"},{"1":"56","2":"1.672522"},{"1":"57","2":"1.672029"},{"1":"58","2":"1.671553"},{"1":"59","2":"1.671093"},{"1":"60","2":"1.670649"},{"1":"61","2":"1.670219"},{"1":"62","2":"1.669804"},{"1":"63","2":"1.669402"},{"1":"64","2":"1.669013"},{"1":"65","2":"1.668636"},{"1":"66","2":"1.668271"},{"1":"67","2":"1.667916"},{"1":"68","2":"1.667572"},{"1":"69","2":"1.667239"},{"1":"70","2":"1.666914"},{"1":"71","2":"1.666600"},{"1":"72","2":"1.666294"},{"1":"73","2":"1.665996"},{"1":"74","2":"1.665707"},{"1":"75","2":"1.665425"},{"1":"76","2":"1.665151"},{"1":"77","2":"1.664885"},{"1":"78","2":"1.664625"},{"1":"79","2":"1.664371"},{"1":"80","2":"1.664125"},{"1":"81","2":"1.663884"},{"1":"82","2":"1.663649"},{"1":"83","2":"1.663420"},{"1":"84","2":"1.663197"},{"1":"85","2":"1.662978"},{"1":"86","2":"1.662765"},{"1":"87","2":"1.662557"},{"1":"88","2":"1.662354"},{"1":"89","2":"1.662155"},{"1":"90","2":"1.661961"},{"1":"91","2":"1.661771"},{"1":"92","2":"1.661585"},{"1":"93","2":"1.661404"},{"1":"94","2":"1.661226"},{"1":"95","2":"1.661052"},{"1":"96","2":"1.660881"},{"1":"97","2":"1.660715"},{"1":"98","2":"1.660551"},{"1":"99","2":"1.660391"},{"1":"100","2":"1.660234"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

The _t_ table functions a little bit differently than the _z_ table. When we use the _z_ table, we find the percentile of the standard normal (Z) distribution by using a specific number of standard deviations.

If, like above, we were trying to look up <span class="math inline">\(z = 6.08\)</span>, we would find that it is above 99.999% of all values in the distribution - this shows that we indeed have a difference greater than the 97.5% needed to show that <span class="math inline">\(p < .05\)</span> so that we can reject our null hypothesis (<span class="math inline">\(H_0\)</span>).

In the case of the _t_ table, we don’t use our _t_ value to look up the percentage (percentile). We actually use our degrees of freedom (<span class="math inline">\(df\)</span>) to look up a level of significance (a _z_ value).

In this example, we have a sample of size <span class="math inline">\(N = 30\)</span> people. This means that our degrees of freedom is <span class="math inline">\(df = N - 1 = 30 - 1 = 29\)</span>. Looking this value up in the table, we find a “significance threshold” of <span class="math inline">\(t = 1.699\)</span>ish.

What this truly means is that, for a sample of size <span class="math inline">\(N = 30\)</span>, a _t_ value of <span class="math inline">\(t = 1.699\)</span> is at that 97.5% mark on the Z distribution (so that <span class="math inline">\(p < .05\)</span>). So as long as our _t_ is greater than <span class="math inline">\(1.699\)</span>, we have found an effect that is significant at the <span class="math inline">\(p < .05\)</span> level; enough to comfortably reject the null hypothesis.

Since our <span class="math inline">\(t = 6.08\)</span>, we reject the null hypothesis (<span class="math inline">\(p < .05\)</span>).

<div id="the-og-question" class="section level3">

### <span class="header-section-number">6.1.1</span> The OG Question

Going back up to almost the start of this tutorial, we had the following null and alternative hypotheses:

<span class="math inline">\(H_0\)</span> = “no population difference in well-being between baseline and post-classical music intervention.” <span class="math inline">\(H_1\)</span> = “there is a population difference in well-being between baseline and post-classical music intervention.”

Well! We have enough stats knowledge to figure this out now. Better still, we can try to use the R programming language to solve this.

We need to calculate the mean and standard deviation of PSYCH 292 students’ well-being scores before the classical music intervention.

**Using R:** We’ll display our data once again. We’ll also use it to calculate some statistics!

    # let's display the participants in the "Classical" condition
    wellbeing_intervention_data %>% filter(Music == "Classical")

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["Participant"],"name":[1],"type":["int"],"align":["right"]},{"label":["Coffees"],"name":[2],"type":["int"],"align":["right"]},{"label":["Music"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Baseline_WB"],"name":[4],"type":["int"],"align":["right"]},{"label":["Post_WB"],"name":[5],"type":["int"],"align":["right"]}],"data":[{"1":"3","2":"3","3":"Classical","4":"4","5":"4"},{"1":"5","2":"4","3":"Classical","4":"5","5":"5"},{"1":"8","2":"3","3":"Classical","4":"4","5":"5"},{"1":"12","2":"3","3":"Classical","4":"5","5":"6"},{"1":"14","2":"3","3":"Classical","4":"5","5":"6"},{"1":"17","2":"3","3":"Classical","4":"4","5":"7"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

    # let's calculate some descriptive statistics that we'll need
    N_classical <- nrow(wellbeing_intervention_data %>% filter(Music == "Classical"))
    M_pretest <- mean((wellbeing_intervention_data %>% filter(Music == "Classical"))$Baseline_WB)
    SD_pretest <- sd((wellbeing_intervention_data %>% filter(Music == "Classical"))$Baseline_WB)
    M_posttest <- mean((wellbeing_intervention_data %>% filter(Music == "Classical"))$Post_WB)
    SD_posttest <- sd((wellbeing_intervention_data %>% filter(Music == "Classical"))$Post_WB)

**Using R:** We can also print these values!

    # let's 'display' these values
    print(N_classical)

    ## [1] 6

    print(M_pretest)

    ## [1] 4.5

    print(SD_pretest)

    ## [1] 0.5477226

    print(M_posttest)

    ## [1] 5.5

    print(SD_posttest)

    ## [1] 1.048809

**Using R:** And calculate our degrees of freedom using another (already existing) variable!

    # let's 'display' these values
    df <- N_classical - 1

    # and we'll print it
    print(df)

    ## [1] 5

Okay. We know that <span class="math inline">\(N\)</span> = 6, <span class="math inline">\(M_1\)</span> = 4.5, <span class="math inline">\(M_2\)</span> = 5.5, <span class="math inline">\(SD_1\)</span> = 0.5477226, <span class="math inline">\(SD_2\)</span> = 1.0488088, and <span class="math inline">\(df\)</span> = 5, so we have all the information we need to do some arithmetic! So let’s calculate our _t_ statistic using these statistics.

<span class="math display">\[t = \frac{M_2-M_1}{\sqrt{\frac{SD_1^2}{N} + \frac{SD_2^2}{N}}} = \frac{5.5-4.5}{\sqrt{\frac{0.5477^2}{6} + \frac{1.0488^2}{6}}} = \frac{1}{0.483} = 2.07\]</span>

**Using R:** We can also use R to calculate this _t_ statistic as follows.

    # calculate t (sqrt is for a square root)
    t <- (M_posttest - M_pretest) / sqrt((SD_pretest^2 + SD_posttest^2) / N_classical)

    # and we'll print the value
    print(t)

    ## [1] 2.070197

We get the same thing a whole lot quicker! Therefore, we find a <span class="math inline">\(t = 2.07\)</span> with <span class="math inline">\(df = 5\)</span>. Let’s look this value up in the _t_ table.

    tbl_t

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["df"],"name":[1],"type":["int"],"align":["right"]},{"label":["significance_value"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"6.313752"},{"1":"2","2":"2.919986"},{"1":"3","2":"2.353363"},{"1":"4","2":"2.131847"},{"1":"5","2":"2.015048"},{"1":"6","2":"1.943180"},{"1":"7","2":"1.894579"},{"1":"8","2":"1.859548"},{"1":"9","2":"1.833113"},{"1":"10","2":"1.812461"},{"1":"11","2":"1.795885"},{"1":"12","2":"1.782288"},{"1":"13","2":"1.770933"},{"1":"14","2":"1.761310"},{"1":"15","2":"1.753050"},{"1":"16","2":"1.745884"},{"1":"17","2":"1.739607"},{"1":"18","2":"1.734064"},{"1":"19","2":"1.729133"},{"1":"20","2":"1.724718"},{"1":"21","2":"1.720743"},{"1":"22","2":"1.717144"},{"1":"23","2":"1.713872"},{"1":"24","2":"1.710882"},{"1":"25","2":"1.708141"},{"1":"26","2":"1.705618"},{"1":"27","2":"1.703288"},{"1":"28","2":"1.701131"},{"1":"29","2":"1.699127"},{"1":"30","2":"1.697261"},{"1":"31","2":"1.695519"},{"1":"32","2":"1.693889"},{"1":"33","2":"1.692360"},{"1":"34","2":"1.690924"},{"1":"35","2":"1.689572"},{"1":"36","2":"1.688298"},{"1":"37","2":"1.687094"},{"1":"38","2":"1.685954"},{"1":"39","2":"1.684875"},{"1":"40","2":"1.683851"},{"1":"41","2":"1.682878"},{"1":"42","2":"1.681952"},{"1":"43","2":"1.681071"},{"1":"44","2":"1.680230"},{"1":"45","2":"1.679427"},{"1":"46","2":"1.678660"},{"1":"47","2":"1.677927"},{"1":"48","2":"1.677224"},{"1":"49","2":"1.676551"},{"1":"50","2":"1.675905"},{"1":"51","2":"1.675285"},{"1":"52","2":"1.674689"},{"1":"53","2":"1.674116"},{"1":"54","2":"1.673565"},{"1":"55","2":"1.673034"},{"1":"56","2":"1.672522"},{"1":"57","2":"1.672029"},{"1":"58","2":"1.671553"},{"1":"59","2":"1.671093"},{"1":"60","2":"1.670649"},{"1":"61","2":"1.670219"},{"1":"62","2":"1.669804"},{"1":"63","2":"1.669402"},{"1":"64","2":"1.669013"},{"1":"65","2":"1.668636"},{"1":"66","2":"1.668271"},{"1":"67","2":"1.667916"},{"1":"68","2":"1.667572"},{"1":"69","2":"1.667239"},{"1":"70","2":"1.666914"},{"1":"71","2":"1.666600"},{"1":"72","2":"1.666294"},{"1":"73","2":"1.665996"},{"1":"74","2":"1.665707"},{"1":"75","2":"1.665425"},{"1":"76","2":"1.665151"},{"1":"77","2":"1.664885"},{"1":"78","2":"1.664625"},{"1":"79","2":"1.664371"},{"1":"80","2":"1.664125"},{"1":"81","2":"1.663884"},{"1":"82","2":"1.663649"},{"1":"83","2":"1.663420"},{"1":"84","2":"1.663197"},{"1":"85","2":"1.662978"},{"1":"86","2":"1.662765"},{"1":"87","2":"1.662557"},{"1":"88","2":"1.662354"},{"1":"89","2":"1.662155"},{"1":"90","2":"1.661961"},{"1":"91","2":"1.661771"},{"1":"92","2":"1.661585"},{"1":"93","2":"1.661404"},{"1":"94","2":"1.661226"},{"1":"95","2":"1.661052"},{"1":"96","2":"1.660881"},{"1":"97","2":"1.660715"},{"1":"98","2":"1.660551"},{"1":"99","2":"1.660391"},{"1":"100","2":"1.660234"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

We notice that a significant _t_ value using only 5 degrees of freedom is <span class="math inline">\(t_{significant} = 2.01\)</span>. Since the _t_ value we calculated meets (and surpasses) this level, we reject the null hypothesis - that is, there is a significant chance that there is a difference between the pretest and posttest well-being samples; the classical music intervention appears to have worked!

This being said, we’re only using <span class="math inline">\(N = 6\)</span> participants. Notice that the more participants we use, the lower the calculated _t_ value needs to be to be significant (<span class="math inline">\(p < .05\)</span>). As we discussed above, this makes sense because the larger our sample, the more we can believe that those standard deviations approach the real deal (the population standard deviations).

In APA-7 format, we might report this finding as follows:

“A significant change in self-report well-being was measured within the <span class="math inline">\(N = 6\)</span> participants between pre-test (<span class="math inline">\(M_1 = 5.5\)</span>, <span class="math inline">\(SD_1 = 0.55\)</span>) and post-test (<span class="math inline">\(M_2 = 4.5\)</span>, <span class="math inline">\(SD_2 = 1.05\)</span>) 7 days later (<span class="math inline">\(t(5) = 2.07\)</span>, <span class="math inline">\(p < .05\)</span>).”

</div>

</div>

<div id="independent" class="section level2">

## <span class="header-section-number">6.2</span> Independent

We’re going to end the tutorial with the second major type of _t_-test we can perform.

1.  Independent Samples _t_-Test

This _t_-test is only slightly different from the above because, instead of looking at within-participant change across time or across intervention, we look at whether two different (independent) groups of participants experience significantly different change.

This type of question is useful if we were to investigate whether the change that is experienced in the Classical-condition participants over time is greater than the change that is naturally experienced in the control condition over time (For example, there may be maturation or history effects left uncontrolled that cause a natural increase in well-being. This aspect of control is one of the beautiful features of an experiment.)

Really, there are only two differences between the dependent and independent samples _t_. First, by design, two independent samples have the chance to be of different sample sizes (<span class="math inline">\(n\)</span>’s).

Therefore, instead of just referring to <span class="math inline">\(n\)</span> in our _t_-statistic formula, we must consider the possibility of an <span class="math inline">\(n_1\)</span> and <span class="math inline">\(n_2\)</span>. Plain and simple.

<span class="math display">\[t = \frac{M_2-M_1}{\sqrt{\frac{SD_1^2}{n_1} + \frac{SD_2^2}{n_2}}}\]</span>

The second difference is that, since we are now considering <span class="math inline">\(n_1 + n_2\)</span> participants in total (instead of only <span class="math inline">\(n\)</span> participants), we must use a different value for our degrees of freedom.

Instead we use <span class="math inline">\(df = (n_1 - 1) + (n_2 - 1)\)</span>. It’s almost the same.

We can then go about calculating our _t_ the same as before. Let’s do just that.

Recall that we already have all the values for the classical music condition:

**Using R:** We’ll print these descriptive statistics a second time!

    # let's 'display' these values
    print(N_classical)

    ## [1] 6

    print(M_pretest)

    ## [1] 4.5

    print(SD_pretest)

    ## [1] 0.5477226

    print(M_posttest)

    ## [1] 5.5

    print(SD_posttest)

    ## [1] 1.048809

**Using R:** We’ll compute the control condition descriptives now.

    # let's display the participants in the "Classical" condition
    wellbeing_intervention_data %>% filter(Music == "Control")

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["Participant"],"name":[1],"type":["int"],"align":["right"]},{"label":["Coffees"],"name":[2],"type":["int"],"align":["right"]},{"label":["Music"],"name":[3],"type":["chr"],"align":["left"]},{"label":["Baseline_WB"],"name":[4],"type":["int"],"align":["right"]},{"label":["Post_WB"],"name":[5],"type":["int"],"align":["right"]}],"data":[{"1":"1","2":"3","3":"Control","4":"4","5":"5"},{"1":"4","2":"1","3":"Control","4":"2","5":"3"},{"1":"6","2":"3","3":"Control","4":"4","5":"5"},{"1":"9","2":"4","3":"Control","4":"4","5":"4"},{"1":"10","2":"2","3":"Control","4":"4","5":"4"},{"1":"16","2":"0","3":"Control","4":"5","5":"4"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

    # let's calculate some descriptive statistics that we'll need
    N_control <- nrow(wellbeing_intervention_data %>% filter(Music == "Control"))
    M_control_t1 <- mean((wellbeing_intervention_data %>% filter(Music == "Control"))$Baseline_WB)
    SD_control_t1 <- sd((wellbeing_intervention_data %>% filter(Music == "Control"))$Baseline_WB)
    M_control_t2 <- mean((wellbeing_intervention_data %>% filter(Music == "Control"))$Post_WB)
    SD_control_t2 <- sd((wellbeing_intervention_data %>% filter(Music == "Control"))$Post_WB)

**Using R:** We’ll print the control descriptive statistics now too!

    # let's 'display' these values
    print(N_control)

    ## [1] 6

    print(M_control_t1)

    ## [1] 3.833333

    print(SD_control_t1)

    ## [1] 0.9831921

    print(M_control_t2)

    ## [1] 4.166667

    print(SD_control_t2)

    ## [1] 0.7527727

We need a couple intermediate values to make this work. We’re looking at the _change_ in means, so let’s ensure we’re using those values.

    # let's 'display' these values
    M_classical_change <- M_posttest - M_pretest
    M_control_change <- M_control_t2 - M_control_t1
    SD_classical_change <- sqrt((SD_pretest^2 + SD_posttest^2) / N_classical)
    SD_control_change <- sqrt((SD_control_t1^2 + SD_control_t2^2) / N_control)

From this, we figure out that the mean change in scores for classical participants was 1, while mean change in the control condition was 0.3333333\. We also found the standard deviation of these changes; 0.4830459 and 0.505525, respectively.

**Using R:** Okay. We’ll compute the independent _t_-value now. Only this time, we’re going to rely completely on R; we won’t be calculating it by hand.

    # calculate t (sqrt is for a square root)
    t <- (M_classical_change - M_control_change) /
      sqrt((SD_classical_change^2 / N_classical)
             + (SD_control_change^2 / N_control))

    # and we'll print the value
    print(t)

    ## [1] 2.335497

Therefore, we find a <span class="math inline">\(t = 2.335\)</span> with <span class="math inline">\(df = (6 - 1) + (6 - 1) = 10\)</span>. Let’s look this value up in the _t_ table.

    tbl_t

<div data-pagedtable="false"><script data-pagedtable-source="" type="application/json">{"columns":[{"label":["df"],"name":[1],"type":["int"],"align":["right"]},{"label":["significance_value"],"name":[2],"type":["dbl"],"align":["right"]}],"data":[{"1":"1","2":"6.313752"},{"1":"2","2":"2.919986"},{"1":"3","2":"2.353363"},{"1":"4","2":"2.131847"},{"1":"5","2":"2.015048"},{"1":"6","2":"1.943180"},{"1":"7","2":"1.894579"},{"1":"8","2":"1.859548"},{"1":"9","2":"1.833113"},{"1":"10","2":"1.812461"},{"1":"11","2":"1.795885"},{"1":"12","2":"1.782288"},{"1":"13","2":"1.770933"},{"1":"14","2":"1.761310"},{"1":"15","2":"1.753050"},{"1":"16","2":"1.745884"},{"1":"17","2":"1.739607"},{"1":"18","2":"1.734064"},{"1":"19","2":"1.729133"},{"1":"20","2":"1.724718"},{"1":"21","2":"1.720743"},{"1":"22","2":"1.717144"},{"1":"23","2":"1.713872"},{"1":"24","2":"1.710882"},{"1":"25","2":"1.708141"},{"1":"26","2":"1.705618"},{"1":"27","2":"1.703288"},{"1":"28","2":"1.701131"},{"1":"29","2":"1.699127"},{"1":"30","2":"1.697261"},{"1":"31","2":"1.695519"},{"1":"32","2":"1.693889"},{"1":"33","2":"1.692360"},{"1":"34","2":"1.690924"},{"1":"35","2":"1.689572"},{"1":"36","2":"1.688298"},{"1":"37","2":"1.687094"},{"1":"38","2":"1.685954"},{"1":"39","2":"1.684875"},{"1":"40","2":"1.683851"},{"1":"41","2":"1.682878"},{"1":"42","2":"1.681952"},{"1":"43","2":"1.681071"},{"1":"44","2":"1.680230"},{"1":"45","2":"1.679427"},{"1":"46","2":"1.678660"},{"1":"47","2":"1.677927"},{"1":"48","2":"1.677224"},{"1":"49","2":"1.676551"},{"1":"50","2":"1.675905"},{"1":"51","2":"1.675285"},{"1":"52","2":"1.674689"},{"1":"53","2":"1.674116"},{"1":"54","2":"1.673565"},{"1":"55","2":"1.673034"},{"1":"56","2":"1.672522"},{"1":"57","2":"1.672029"},{"1":"58","2":"1.671553"},{"1":"59","2":"1.671093"},{"1":"60","2":"1.670649"},{"1":"61","2":"1.670219"},{"1":"62","2":"1.669804"},{"1":"63","2":"1.669402"},{"1":"64","2":"1.669013"},{"1":"65","2":"1.668636"},{"1":"66","2":"1.668271"},{"1":"67","2":"1.667916"},{"1":"68","2":"1.667572"},{"1":"69","2":"1.667239"},{"1":"70","2":"1.666914"},{"1":"71","2":"1.666600"},{"1":"72","2":"1.666294"},{"1":"73","2":"1.665996"},{"1":"74","2":"1.665707"},{"1":"75","2":"1.665425"},{"1":"76","2":"1.665151"},{"1":"77","2":"1.664885"},{"1":"78","2":"1.664625"},{"1":"79","2":"1.664371"},{"1":"80","2":"1.664125"},{"1":"81","2":"1.663884"},{"1":"82","2":"1.663649"},{"1":"83","2":"1.663420"},{"1":"84","2":"1.663197"},{"1":"85","2":"1.662978"},{"1":"86","2":"1.662765"},{"1":"87","2":"1.662557"},{"1":"88","2":"1.662354"},{"1":"89","2":"1.662155"},{"1":"90","2":"1.661961"},{"1":"91","2":"1.661771"},{"1":"92","2":"1.661585"},{"1":"93","2":"1.661404"},{"1":"94","2":"1.661226"},{"1":"95","2":"1.661052"},{"1":"96","2":"1.660881"},{"1":"97","2":"1.660715"},{"1":"98","2":"1.660551"},{"1":"99","2":"1.660391"},{"1":"100","2":"1.660234"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}</script></div>

For <span class="math inline">\(df = 10\)</span>, we only need <span class="math inline">\(t_{significant} = 1.812\)</span> for us to have a significant effect. Since <span class="math inline">\(t = 2.335\)</span>, we find that <span class="math inline">\(t > t_{significant}\)</span> and therefore we reject the null hypothesis that the change in well-being among classical participants was due to maturation and/or history.

Therefore, we believe the classical music intervention leads to an improvement in the well-being of PSYCH 292 students.

</div>

</div>

<div id="conclusion" class="section level1">

# <span class="header-section-number">7</span> Conclusion

This was a fun tutorial, but we’ve only scratched the surface of _t_-tests, statistics, and the R language.

There’s also many more analyses that could be run on these data.

I hope you got something out of this! Thanks for joining me :))

(I’m going to go to bed now because running stats makes me lose track of time; it’s 3AM and is therefore past my bedtime.)

</div>

</div>

</div>

</div>

<script>// add bootstrap table styles to pandoc tables function bootstrapStylePandocTables() { $('tr.header').parent('thead').parent('table').addClass('table table-condensed'); } $(document).ready(function () { bootstrapStylePandocTables(); });</script> <script>$(document).ready(function () { window.buildTabsets("TOC"); }); $(document).ready(function () { $('.tabset-dropdown > .nav-tabs > li').click(function () { $(this).parent().toggleClass('nav-tabs-open') }); });</script> <script>$(document).ready(function () { // move toc-ignore selectors from section div to header $('div.section.toc-ignore') .removeClass('toc-ignore') .children('h1,h2,h3,h4,h5').addClass('toc-ignore'); // establish options var options = { selectors: "h1,h2,h3", theme: "bootstrap3", context: '.toc-content', hashGenerator: function (text) { return text.replace(/[.\\/?&!#<>]/g, '').replace(/\s/g, '_').toLowerCase(); }, ignoreSelector: ".toc-ignore", scrollTo: 0 }; options.showAndHide = true; options.smoothScroll = true; // tocify var toc = $("#TOC").tocify(options).data("toc-tocify"); });</script> <script>(function () { var script = document.createElement("script"); script.type = "text/javascript"; script.src = "https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"; document.getElementsByTagName("head")[0].appendChild(script); })();</script>