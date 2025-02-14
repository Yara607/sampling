# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Yalda Rahmati

```
Please write your explanation here...
Part 1:
•	Sampling occurs in three stages: (1) Random infection assignment, (2) Primary contact tracing, and (3) Secondary event-based tracing.
•	The model uses binomial random sampling for both infection and tracing.
•	Bias occurs because larger events are more likely to trigger full tracing, leading to an overrepresentation of weddings in the observed data.
•	This matches the findings in the article: the observed data exaggerates the role of weddings as sources of infection.
 Below, I describe the key stages where sampling occurs and the related distributions.
The model starts by creating a population of 1000 individuals, where:
o	200 individuals attend a wedding.
o	800 individuals attend a brunch.
•	This population is represented as a DataFrame (ppl)with columns indicating event type, infection status, and tracing status.
•	This defines the sampling frame.
1. Infect a random subset of people (First Random Sampling)
•	The model assumes a 10% infection rate (ATTACK_RATE = 0.10).
•	The function np.random.choice(ppl.index, size=int(len(ppl) * ATTACK_RATE), replace=False) selects a random 10% of the population to be infected.
•	Since infection is assigned independently for each person, this follows a binomial distribution, meaning each person has a 10% chance of being infected.
2. Primary Contact Tracing (Second Random Sampling)
•	Among the infected individuals, only 20% are successfully traced (TRACE_SUCCESS = 0.20).
•	This is done using the function:
ppl.loc[ppl['infected'], 'traced'] = np.random.rand(sum(ppl['infected'])) < TRACE_SUCCESS
•	This generates a random number between 0 and 1 for each infected person.
•	If the number is less than 0.2, the individual is marked as traced.
•	Since each infected person is traced independently with a fixed probability, this also follows a binomial distribution (with success probability 0.2).
3. Secondary Contact Tracing (Event-Based Sampling)
•	If at least two infected individuals from the same event are successfully traced, all infected attendees of that event are then traced(not randomly)

Part 2:
“whitby_covid_tracing.py” produces a single histogram plot with two overlaid distributions:
•	The blue histogram represents the true proportion of infections from weddings.
•	The red histogram represents the  traced infections from weddings.
The red distribution is shifted to the right, meaning that the traced cases over represent weddings as a source of infection. This aligns with the blog’s conclusion that contact tracing creates a biased view, making weddings appear more significant in the spread of infections than they actually are.

part3:
With 100 repetitions, the results change each time due to more randomness. The graphs look different, but the pattern of weddings being overrepresented stays the same.

part 4: 
I used "np.random.seed(42)" in order to make the code reproducible and it generates the same random samples, making the output consistent across multiple runs. 

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 16/02/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
