# Sanskrit AI Benchmark

This folder is a JSON-first framework for testing AI performance in specialized Sanskrit fields. It includes a v0.1 pilot benchmark with 20 questions across Vyakarana, Nyaya, Mimamsa, and Vedanta.

## Roles

- Benchmark author: creates question files and optional answer-key files.
- Student AI: receives only the question files listed in `student_packet.json`.
- Grader AI: receives the student attempt, `grading/grader_instructions.json`, `benchmark/grading_map.json`, optional answer keys, and reference texts.

## Privacy Boundary

Do not give the student AI these folders:

- `benchmark/answer_keys`
- `benchmark/grading_map.json`
- `reference_texts`
- `grading`

The student AI should see only:

- `benchmark/manifest.json`
- `benchmark/questions`
- `student_packet.json`

## ID Conventions

- Question IDs: `SUBJ-0001`, for example `VYAK-0001`.
- Answer-key IDs: `AK-SUBJ-0001`, usually one per question when a key exists.
- Reference text IDs: `REF-SUBJ-SHORTNAME`.
- Passage IDs: `REF-SUBJ-SHORTNAME:section-or-verse`.
- Attempt IDs: `ATTEMPT-YYYYMMDD-model-slug`.
- Grade run IDs: `GRADE-YYYYMMDD-grader-model-slug`.

## Workflow

1. Add benchmark questions under `benchmark/questions/<subject>/`.
2. Add optional answer keys under `benchmark/answer_keys/<subject>/`.
3. Add authoritative source passages under `reference_texts/<subject>/`.
4. Link questions to keys and reference passages in `benchmark/grading_map.json`.
5. Give only the student packet to the student AI.
6. Store the student AI response under `attempts/<attempt_id>/answers.json`.
7. Run a grader AI with the attempt, grading map, answer keys, and reference texts.
8. Store grading output under `grading/runs/<grade_run_id>/grade_report.json`.

## Pilot Contents

- Questions: 20 total, 5 per subject.
- Answer keys: 20 total, stored separately from student-visible questions.
- Reference passages and grader notes: 22 total.
- Total possible score: 214.
- Student-visible files: `student_packet.json`, `benchmark/manifest.json`, and `benchmark/questions`.
- Grader-only files: `benchmark/answer_keys`, `benchmark/grading_map.json`, `reference_texts`, and `grading`.
