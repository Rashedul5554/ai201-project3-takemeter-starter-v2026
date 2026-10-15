# Current next steps

Completed: 200-entry main dataset, initial training, three before-seeds, baseline, and an AI-generated 30-row staff-label file. Main model results remain unchanged.

Required external input: obtain the authentic data/staff_labels.csv from the teaching fellow. Disclose that AI assigned your comparison labels and confirm whether this is acceptable for the exercise. No model-generated file can substitute for the authentic staff key.

Once the authentic file is available, run from this repository with the project environment active:
python agreement.py

The resulting number is AI–staff agreement given the current provenance. Record that distinction when writing disagreement adjudications. After that, choose one supported improvement, set RUN_LABEL = "after", rerun the three-seed cell, and preserve the existing before file.

Commit the new staff CSV, staff_labeling_notes.md, and updated README. Do not claim an independent-labeling commit. Three personal dataset hard cases and unresolved provenance/suitability reviews remain incomplete in README; no personal experiences or staff results were invented.
