# C Class Data Extraction Schema

## Goal
Extract all C class (50_CSc) content into lossless JSON format for AI consumption via CLI.

## Key Variables for CLI AI Query

```bash
# Base paths
COURSE_ROOT="/Users/trevabbott/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc"
ASSIGNMENTS_DIR="${COURSE_ROOT}/assignments"
TEMPLATES_DIR="/Users/trevabbott/bayFamilyVault/schoolMediaTrev/00_templates"

# Assignment categories (detected from paths)
HW_ASSIGNMENTS="001_hw 002_hw 003_hw 004_hw 005_hw 006_hw 007_HW"
QUIZ_ASSIGNMENTS="001_quiz 002_quiz"
ARRAY_CONWAY="03_arrays_conway"
MIDTERM="004a_midterm 004b"
FINAL="final"
CHECKIN="00_checkIn"

# File types to extract
CODE_FILES="*.c *.h"
DOC_FILES="*.md *.txt *.pdf"
```

## JSON Schema Structure

```json
{
  "course": {
    "id": "50_CSc",
    "name": "Intro to C Programming",
    "institution": "SMC",
    "semester": "UNKNOWN",
    "instructor": "UNKNOWN",
    "base_path": "/Users/trevabbott/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc"
  },
  "assignments": [
    {
      "id": "001_hw",
      "type": "homework",
      "number": 1,
      "problems": [
        {
          "id": "01_case_area",
          "name": "Case Area Calculator",
          "path": "assignments/001_hw/01_case_area",
          "files": {
            "source": ["src/main.c"],
            "headers": [],
            "docs": [],
            "notes": ""
          },
          "metadata": {
            "lines_of_code": 0,
            "has_iterations": false,
            "status": "complete"
          }
        }
      ]
    }
  ],
  "templates": [
    {
      "path": "/Users/trevabbott/bayFamilyVault/schoolMediaTrev/00_templates/00HW/001/practice.c",
      "type": "homework_template",
      "assignment_ref": "001_hw"
    }
  ],
  "duplicates": {
    "guessing_game": [
      "002_hw/05_guessing_game/src/main.c",
      "002_hw/05_guessing_game copy/src/main.c"
    ],
    "guessing_game_loop": [
      "002_hw/06_guessing_game_loop/src/main.c",
      "002_hw/06_guessing_game_loop copy/src/main.c"
    ]
  },
  "iterations": {
    "bitcoin_simulator": [
      "005_hw/bitcoin_simulator/main.c",
      "005_hw/bitcoin_simulator/bit/main.c",
      "005_hw/bitcoin_simulator/draft2/main.c",
      "005_hw/bitcoin_simulator/draft2/test2/main.c"
    ],
    "conway": [
      "03_arrays_conway/main.c",
      "03_arrays_conway/03_HW/main.c",
      "03_arrays_conway/src/main.c"
    ],
    "midterm": [
      "004a_midterm/main.c",
      "004a_midterm/src/main.c",
      "004a_midterm/src/.vscode/mid2/main.c",
      "004a_midterm/src/.vscode/mid2/mid3/main.c"
    ]
  },
  "final_exam": {
    "problems": [
      "final/001/main.c",
      "final/002/main.c",
      "...",
      "final/020/main.c"
    ],
    "total_problems": 20
  }
}
```

## CLI AI Extraction Command Template

```bash
# Step 1: Extract all file paths
find "$COURSE_ROOT" -type f \( -name "*.c" -o -name "*.h" -o -name "*.md" \) > c_class_files.txt

# Step 2: Generate JSON with file contents
cat << 'JSONGEN' > extract_c_class.sh
#!/bin/bash
COURSE_ROOT="$1"
echo '{'
echo '  "course": {'
echo '    "id": "50_CSc",'
echo '    "base_path": "'$COURSE_ROOT'",'
echo '    "extracted_date": "'$(date -u +"%Y-%m-%dT%H:%M:%SZ")'"'
echo '  },'
echo '  "assignments": ['

# Find all assignments
find "$COURSE_ROOT/assignments" -mindepth 1 -maxdepth 1 -type d | sort | while IFS= read -r assignment_dir; do
  assignment_id=$(basename "$assignment_dir")
  echo '    {'
  echo '      "id": "'$assignment_id'",'
  echo '      "problems": ['
  
  # Find all problem directories
  find "$assignment_dir" -mindepth 1 -maxdepth 1 -type d | sort | while IFS= read -r problem_dir; do
    problem_id=$(basename "$problem_dir")
    echo '        {'
    echo '          "id": "'$problem_id'",'
    echo '          "path": "'${problem_dir#$COURSE_ROOT/}'",'
    echo '          "files": {'
    
    # Extract .c files
    echo '            "c_files": ['
    find "$problem_dir" -name "*.c" -type f | while IFS= read -r cfile; do
      echo '              {'
      echo '                "filename": "'$(basename "$cfile")'",'
      echo '                "relative_path": "'${cfile#$COURSE_ROOT/}'",'
      echo '                "lines": '$(wc -l < "$cfile")','
      echo '                "content": '$(jq -Rs . < "$cfile")
      echo '              },'
    done
    echo '            ],'
    
    # Extract .h files
    echo '            "h_files": ['
    find "$problem_dir" -name "*.h" -type f | while IFS= read -r hfile; do
      echo '              {'
      echo '                "filename": "'$(basename "$hfile")'",'
      echo '                "content": '$(jq -Rs . < "$hfile")
      echo '              },'
    done
    echo '            ]'
    
    echo '          }'
    echo '        },'
  done
  
  echo '      ]'
  echo '    },'
done

echo '  ]'
echo '}'
JSONGEN

chmod +x extract_c_class.sh
```

## Simplified Version (Practical)

For immediate use with CLI AI, use this variable set:

```bash
COURSE_DIR="10_codeLibrary/C_++/50_CSc/assignments"
ASSIGNMENT_TYPES="hw quiz midterm final arrays"
EXTRACT_EXTENSIONS="c h md txt"
MAX_FILE_SIZE_KB=100  # Skip large files
INCLUDE_FILE_CONTENT=true  # Set to false for structure only
```

## Query Template for CLI AI

```
Extract all C programming assignments from path: ${COURSE_ROOT}

Include:
- Assignment structure (homework #, quiz #, midterm, final)
- Problem names and directories
- All .c and .h file contents
- Documentation files (.md, .txt)
- Identify duplicates (files with " copy" suffix)
- Identify iterations (draft2, test2, src/.vscode/mid2)

Output as JSON with structure:
{
  "assignments": [{
    "id": "001_hw",
    "problems": [{
      "id": "01_case_area",
      "files": [{"name": "main.c", "content": "..."}]
    }]
  }]
}

Exclude:
- Binary files (.o, .out, .dSYM)
- System files (.DS_Store)
- Editor configs (.vscode, except content)

Compress by:
- Removing excessive whitespace
- Combining related iterations
- Deduplicating identical files
```

## Usage

```bash
# In your terminal with CLI AI:
ai "$(cat /path/to/C-CLASS-EXTRACTION-SCHEMA.md) - Extract C class at /Users/trevabbott/bayFamilyVault/schoolMediaTrev/10_codeLibrary/C_++/50_CSc and output JSON"
```

This gives the AI all variables needed to parse the directory structure intelligently.
