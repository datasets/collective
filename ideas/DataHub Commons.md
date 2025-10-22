---
created: 2025-02-22
---

Datahub Commons is a wiki-like effort for collaboratively curating data - archiving, documenting and refining datasets. Curation will often be early-stage and data and documentation may be stubbed and incomplete.

It enables open data collaboration and fosters iterative refinement of datasets.

Informally it is about just being able to publish and stub datasets that aren't yet polished. 😊 

### How does this relate to DataHub core?

I want to distinguish this from what's in Datahub Core, because it the idea of just being able to publish and stub datasets that aren't yet polished.

So I think there's something about items in Commons possibly being very incomplete, which we don't really want for Core datasets. It gives them a bad reputation. Things could maybe move into Core datasets over time.

So there's something of the Wiki-like, Commons-like nature, and also it's very explicit that one is kind of representing or stubbing other datasets or archiving them. So you're not going to be claiming ownership of them.

## Refined version

Datahub Core

- High-quality, polished datasets curated and maintained by Datahub.
-	More authoritative, akin to how Our World in Data handles its datasets—Datahub is responsible for a version of the dataset.
- Aimed at reliability and trust, so incomplete or speculative data doesn’t belong here.
- Datasets can be moved into Core once they reach a certain level of completeness and verification.

Datahub Commons

- A Wiki-like space for stubbing, curating, and linking to datasets from various sources.
- Orientation to stubbing and iteration, allowing early-stage or unstructured datasets to exist without impacting Core’s quality.
- More focused on archiving, referencing, and contextualizing datasets rather than producing a polished version.
- Explicit that Datahub is not responsible (or owning) these datasets but acting as a curation and discovery layer.

Essentially, Commons serves as a sandbox and library, whereas Core is a repository of high-quality, production-ready datasets. This model also allows for an organic pipeline, where datasets can start in Commons, undergo refinement, and move into Core over time.

## FAQs

Where do we store the datasets? Do we use GitHub.com/datasets or another org to avoid confusion with core **my instinct is to use same GitHub org and just distinguish core datasets in some way**

What is connection with collections? **I would merge collections into this in some way**

What is connection with old.datahub.io aka old CKAN.net? **I would migrate everything we can from old datahub with some traffic into commons and then properly deprecate old.datahub.io**

Where would this live url wise? **suggest datahub.io/commons**

How does this relate to the original function and vision of ckan.net? ** It represents its continuation and rebirth 🤩**