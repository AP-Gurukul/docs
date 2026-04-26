# AP Gurukul Question Management & Tagging Strategy

This document outlines the standard operating procedures (SOP) and technical implementation for how questions are tagged, assigned unique IDs, and moved through the review pipeline in the AP Gurukul system.

## 1. Question Lifecycle & Review Workflow

Every question uploaded or created within the admin portal follows a strict status lifecycle to ensure quality control before being presented to students.

### The Status Hierarchy
Questions can hold one of the following statuses (`QuestionStatus`):
- **`draft`**: The initial state when a question is first uploaded from a Word/Excel document or manually created, but not yet sent for review.
- **`review_pending`**: Questions that have been explicitly flagged during upload as needing a secondary review. These questions are isolated in the **Second Review Queue**.
- **`reviewed`**: A temporary state indicating a question has been reviewed by a staff member.
- **`published`**: The final state. Only questions with this status will be available in the main question bank for student consumption.
- **`archived`**: Deprecated or outdated questions that are removed from active circulation but kept for historical records.

### The Second Review Flow
1. **Upload Phase**: During bulk uploading, administrators have two options:
   - *Upload Questions*: Saves questions normally (defaults to `draft`).
   - *Send to Second Review*: Flags the entire batch (or single questions) as `review_pending`.
2. **Review Phase**: The **Second Review** tab filters the database strictly for `status == 'review_pending'`.
3. **Approval**: From the Second Review queue, an admin can click **Approve** on a question card. This instantly promotes the question's status to `published` and removes it from the queue.

---

## 2. Meaningful Question Code Generation

To ensure absolute traceability and prevent duplicates, every question saved to the database is assigned a **Custom Question Code** (`questionCode`).

### Format Structure
The code follows a highly readable yet unique format:
`{SUBJECT_PREFIX}-{TOPIC_PREFIX}-{TIMESTAMP}{RANDOM_CHARS}`

**Example:** `AHC-ANC-LQ8W2XYZ`

### Breakdown of the Code
1. **Subject Prefix (e.g., `AHC`)**
   - Derived from the `subjectId` (e.g., `ap-history-culture`).
   - Extracts the first letter of each word and converts to uppercase.
   - Padded with `X` if the subject is less than 3 words (e.g., `english` -> `EXX`).

2. **Topic Prefix (e.g., `ANC`)**
   - Derived from the `topicId` (e.g., `ancient-history`).
   - Similar to the subject prefix, extracts the first letter of each word.
   - If no topic is assigned, it defaults to `XXX`.

3. **Timestamp Base36 (e.g., `LQ8W2`)**
   - Extracts the last 5 characters of the exact millisecond the question was saved, converted to Base36.
   - This ensures chronological uniqueness.

4. **Random Suffix (e.g., `XYZP`)**
   - A 4-character random alphanumeric string.
   - Guarantees zero collisions during bulk parallel uploads where multiple questions are processed in the exact same millisecond.

*(Note: Difficulty tags like Easy/Medium/Hard are deliberately excluded from the ID structure, as difficulty can fluctuate based on student metrics, and changing an ID based on difficulty is counter-productive.)*

---

## 3. Implementation Rules & Best Practices

### Assignment Timing
- **Temporary UI State**: When questions are parsed from Word/Excel documents, they are assigned a temporary React UI key (e.g., `parsed-17208...`). These are **hidden** from the user.
- **Permanent Assignment**: The actual `questionCode` is **only** generated at the exact moment the payload is sent to Firestore (via `addQuestion` or `batchAddQuestions`). This allows admins to safely tweak subjects and topics *before* uploading without polluting the ID sequence.

### Auto-Regeneration on Updates
- **Firestore IDs vs. Question Codes**: Firestore requires document IDs to be immutable. To allow flexibility, we use standard Firestore Auto-IDs for the physical database document, but store our meaningful structure in the `questionCode` field.
- **Subject/Topic Changes**: If an admin edits a previously saved question and changes its `subjectId` or `topicId`, the backend service automatically intercepts this update and **regenerates a completely new `questionCode`** matching the new subject domain. The old code is abandoned naturally.

### Database Querying
When querying for questions, systems should rely on the actual `subjectId` and `topicId` fields rather than parsing the `questionCode`, as the fields are properly indexed by Firestore. The `questionCode` is primarily for human communication, support tickets, and bulk export traceability.
