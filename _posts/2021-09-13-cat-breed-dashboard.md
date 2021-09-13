---
layout: post
title: "Cat Breed Dashboard: Tableau Public"
---
<iframe src="https://public.tableau.com/views/CatBreedDashboard/CatBreedDashboard?:showVizHome=no&:embed=true" width="1000" height="800"></iframe>
## Why Cats?
I wanted to try my hand at an interactive Tableau dashboard, so I turned to one of my favorite things: cats!  I have four cats who were all rescued from shelters, and it is fun to try to guess at their origins.  I wanted to put everything we know about cat breeds into a single dashboard to allow the user to see which breeds match their desired characteristics.

## Data Source
I chose to compile cat breed data from the [Cat Fancier’s Association (CFA)](https://cfa.org/breeds/).  They recognize 45 pedigreed breeds and include facts about each type on their website.  Unfortunately, the full dataset that I wanted was not available in a structured form.  The main “Breed” page provided a standardized “Coat Length” field, but I had to manually extract other desired details by reading each breed’s individual page.  In an ideal world, I would have built a web scraper to extract my data, but due to the longform structure of the breed pages, the most accurate way to create my dataset was to compile it by hand.

## Handling Ambiguity
As “Coat Length” was the only available structured field, I had to make some choices around how to standardize my data.  CFA’s website gave 3 options for coat length: longhair, longhair and shorthair, and shorthair.  However, I wanted to add a distinction between shorthair cats and “hairless” cats, as there is a massive difference between an American Shorthair and a Sphinx.

When standardizing the origin fields (“Breed Origin” and “Region of Origin”), it is important to keep in mind that cat breeds are artificial.  Cats evolved on their own throughout history, and humans have now defined certain characteristics as “breeds” and have even intentionally crossed different cats to produce desired characteristics.  This makes these fields much more open to interpretation than a measurable characteristic like “Coat Length”.

“Breed Origin” was a challenging field to define.  Eventually, I decided to separate breeds into ones that had evolved over time versus ones with a documented history of intentional crossing by humans.  Some of the breeds, such as the Korat, are documented in medieval manuscripts and therefore clearly defined as a “natural” breed.  Others, such as the Bengal, were originally selectively bred and are therefore classified as “hybrid” even though the breed has now developed to the point where breeding Bengals together produces Bengals.

“Region of Origin” provided the most ambiguity due to the fact that cats have no written history and therefore cannot tell us where they came from.  Many of the natural breeds were easier to define as they have been around long enough to show up in human records.  However, it is also important to define what “origin” means.  For natural breeds, a cat breed may have originated in a certain region but migrated over time.  For hybrid breeds, does “origin” refer to where the breed was created for the first time, or does it refer to the ancestry of the hybrid’s parents?  As the latter is nearly impossible to determine, I chose to define hybrid origin based on the documented first appearance of the breed from the CFA website.

Additionally, I chose to use region rather than country of origin because some ancient breeds are impossible to trace to a particular country.  However, this introduces additional ambiguity.  How do we define geographic regions?  If we use continents, we run into the fact that both Russia and Turkey span both Europe and Asia.  Trying to break down continents into sub-regions runs into political conflicts and ambiguity.  In the end, I chose to define region based on the language used on the CFA website.  They did not break down Europe or North America into sub-regions, but they did have several categories for Asia and the Middle East.

## Visualization Choices
I wanted to create an interactive dashboard where a user could filter for breeds matching the desired characteristics across multiple fields.  To accomplish this, I wanted a visualization that was easy to use rather than visually complex.  This is why both “Breed Origin” and “Coat Length” appear as bar graphs.  In a perfect world, I would have preferred to visualize “Coat Length” as a Venn Diagram, but Tableau does not currently offer this capability.

For “Region of Origin”, I considered plotting it on a map, but Tableau’s auto-generated coordinates are specific to country rather than region.  Since the category is so ambiguous, I chose to keep it simple and use a heatmap visualization because it provides easily clickable categories as well as a size difference that shows the proportion of breeds in one region versus another.