# Dataset notes

## Plan

Three image categories:

| Category | Images planned |
|----------|---------------:|
| Normal | 50 |
| Broken | 50 |
| Scratch | 50 |
| **Total** | **150** |

When the data is added, the planned folders are `dataset/normal/`, `dataset/broken/` and `dataset/scratch/`.

## AITEX textile defect dataset

The AITEX dataset was downloaded and image tiles were prepared for the initial experiments. It was used for the initial **binary** CNN (validation accuracy about 85.96%, with weak defect detection in the confusion matrix).

It is **not included** in this repository. Check the dataset's own terms and the file sizes before committing any of its images or tiles.

## Synthetic samples

The 50-image PDFs generated for the project contain **synthetic, AI-generated texture samples**. They are **not** verified real fabric photographs.

- They must not be presented as real production data.
- If they are ever added to the repository, they must be clearly labelled as synthetic.

## What is still needed

- Genuine, correctly labelled photographs of normal, broken and scratched fabric
- Unseen real fabric kept for testing the three-class model
