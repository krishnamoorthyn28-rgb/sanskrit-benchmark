# Sanskrit AI Benchmark Design Spec

## Purpose

This framework evaluates AI systems on specialized Sanskrit knowledge through blind exam attempts and source-grounded grading. It separates the benchmark author's materials, the student AI's answers, and the grader AI's evidence so that each role has the right level of access.

## Core Objects

- Questions: student-visible prompts identified by stable `question_id` values such as `VYAK-0001`.
- Answer keys: optional grader-only rubrics identified by `answer_key_id` values and linked to questions.
- Reference texts: authoritative grader-only passages organized by subject area and cited by `passage_id`.
- Attempts: one folder per student AI run, containing metadata and answers keyed by `question_id`.
- Grade reports: one folder per grading run, containing scores, comments, evidence passage IDs, and summary statistics.

## Folder Model

- `benchmark/questions/<subject>/`: blind exam paper files safe for student AIs.
- `benchmark/answer_keys/<subject>/`: optional answer keys and rubrics.
- `benchmark/grading_map.json`: private map linking questions to answer keys, reference passages, grading mode, and max score.
- `reference_texts/<subject>/`: grader-only source corpus organized by Sanskrit discipline.
- `attempts/<attempt_id>/`: student AI responses and attempt metadata.
- `grading/`: grader instructions and grading outputs.
- `schemas/`: JSON schemas for validation and interoperability.

## Access Rules

The student AI receives only `student_packet.json`, `benchmark/manifest.json`, and `benchmark/questions`. It must not receive answer keys, reference texts, grading maps, source hints, book lists, search access, or external tools.

The grader AI receives the student attempt plus all grader-only materials: answer keys, reference texts, grading map, and grading instructions.

## Grading Modes

- `answer_key_only`: grade against a fixed rubric or reference answer.
- `reference_text_only`: grade by comparing the answer to mapped authoritative passages.
- `answer_key_plus_reference_text`: use both rubric and source evidence.

Answer keys are optional because some Sanskrit questions require interpretive or source-grounded judgment rather than one fixed correct answer.

## Workflow

1. The benchmark author writes questions and optional answer keys.
2. The author adds or updates authoritative reference passages.
3. The author links each question to its grading evidence in `grading_map.json`.
4. A student AI takes the exam blind and writes `attempts/<attempt_id>/answers.json`.
5. A grader AI scores the attempt using the private grading materials.
6. The grade report records scores, comments, error tags, and cited evidence.

## Design Goals

- Preserve blind evaluation integrity.
- Keep all data portable as plain JSON.
- Make question, answer, attempt, and grading records joinable by stable IDs.
- Support both objective rubric grading and open-ended source-grounded grading.
- Organize Sanskrit materials by discipline so the corpus can grow cleanly.

