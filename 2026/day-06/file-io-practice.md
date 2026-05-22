<!-- @format -->

# File I/O Practice

## Commands run

1. `touch notes.txt`
   - Created an empty file named `notes.txt`.

2. `echo "Line 1: DevOps file I/O practice" > notes.txt`
   - Wrote the first line to the file using output redirection.

3. `echo "Line 2: Created, appended, and read this file" >> notes.txt`
   - Appended the second line without overwriting the existing content.

4. `echo "Line 3: Verified content with cat, head, and tail" | tee -a notes.txt`
   - Added the third line and displayed it at the same time using `tee`.

5. `cat notes.txt`
   - Read the full file contents to confirm all three lines were present.

6. `head -n 2 notes.txt`
   - Displayed the first two lines to verify the file start.

7. `tail -n 2 notes.txt`
   - Displayed the last two lines to verify appended content.

## Observations

- `touch` created the file without any content.
- `>` replaced the file content, while `>>` safely appended new text.
- `tee -a` allowed writing and viewing output at the same time.
- `cat`, `head`, and `tail` each provided a quick way to inspect file contents.

## Resulting file

- Created `notes.txt` with 3 lines of practice text.
- Confirmed the file content with `cat`, `head`, and `tail`.
