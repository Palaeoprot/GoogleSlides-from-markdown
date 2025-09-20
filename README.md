# Google Slides from Markdown Converter

This project provides a Google Colab notebook that converts a specially formatted Markdown file into a Google Slides presentation. It uses a predefined Google Slides template, allowing for consistent branding and layout while enabling content creation in a simple, text-based format.

This tool is designed for both direct use and as a teaching resource for students learning to interact with Google APIs in Python.

## Features

  - **Template-Based Creation**: Creates new presentations by copying a master Google Slides template.
  - **Markdown-Driven Content**: Parses a Markdown file with YAML-style frontmatter to define each slide's content and layout.
  - **Structured Layouts**: Supports different slide layouts (e.g., Title, Content, Section Header) mapped directly to your template's master slides.
  - **Automated Folder Management**: Automatically creates the specified save folder in your Google Drive if it doesn't exist.
  - **Google Colab Integration**: Seamlessly handles authentication and Google Drive mounting within the Colab environment.

-----

## Requirements

### 1\. Software & Libraries

The script is designed to run in a Google Colab environment and requires the following Python libraries:

  - `google-colab`
  - `google-api-python-client`
  - `PyYAML`

These are pre-installed or easily installed in Colab.

### 2\. Google Account & API Access

You will need:

  - A Google Account.
  - **Google Drive API** and **Google Slides API** enabled for your account. You can enable them here: [Google Cloud Console](https://console.cloud.google.com/apis/dashboard).
  - A Google Slides file to use as a template. It must be accessible by the authenticated user (e.g., saved in your Google Drive).

-----

## Setup and Configuration

Before running the notebook, you need to configure the parameters in the third code cell.

```python
# ===== Cell 3 =====
# Configuration parameters
md_path = "/content/your_presentation.md"  #@param {type:"string"}
save_folder = "/content/drive/MyDrive/Presentations/" #@param {type:"string"}
template_id = "YOUR_TEMPLATE_ID_HERE" #@param {type:"string"}
presentation_name = "My New Presentation" #@param {type:"string"}
debug_mode = False #@param {type:"boolean"}
```

1.  **`md_path`**: The path to the markdown file you want to convert. You must upload this file to your Colab environment.
2.  **`save_folder`**: The path within your Google Drive where the new presentation will be saved. The script will create this folder structure if it does not exist.
3.  **`template_id`**: The ID of your Google Slides template. To find it, open your template in Google Slides and copy the ID from the URL:
    `https://docs.google.com/presentation/d/`**`THIS_IS_THE_ID`**`/edit`
4.  **`presentation_name`**: The file name for the generated Google Slides presentation.
5.  **`debug_mode`**: Set to `True` to print additional parsing information.

-----

## Usage Guide

1.  **Prepare Your Markdown**: Create a `.md` file following the formatting rules below.
2.  **Upload Markdown**: Upload your file to the Colab environment (using the file browser on the left).
3.  **Configure Parameters**: Update the variables in **Cell 3** of the notebook with your file paths, template ID, and desired presentation name.
4.  **Run All Cells**: Execute the cells in the notebook sequentially.
      - **Cell 2** will prompt you to authenticate with your Google account and grant access to Drive.
      - The script will process the markdown, create the presentation, and print a direct link to the new Google Slides file upon completion.

-----

## Markdown Formatting Rules

The script uses a strict format to parse slides. Each slide is a block of YAML-style key-value pairs, separated by `---`.

### Slide Separator

  - Each slide section **must** be separated by `---` on its own line.

### Slide Structure

Each section defines one slide and uses the following fields:

  - `layout:` (Required): The layout to use. Must match a layout name in your template. Common values are `title`, `content`, and `section_header`.
  - `title:` (Required): The main title of the slide.
  - `subtitle:` (Optional): A subtitle, typically used for `title` and `section_header` layouts.
  - `presenter:` (Optional): The presenter's name, used on `title` slides.
  - `body:` (Optional): The main content for the slide. **Must** use the pipe `|` character for multi-line content.

### Body Content

  - The `body` field must start with `body: |`.
  - Every subsequent line of body content **must be indented with exactly 2 spaces**.
  - Use standard Markdown for bullet points (`*` or `-`), bold (`**text**`), and italics (`*text*`).

### Example Markdown File

```markdown
layout: title
title: My Presentation Title
subtitle: A Comprehensive Overview
presenter: Your Name
---
layout: section_header
title: Introduction
subtitle: Setting the Context
---
layout: content
title: Key Concepts
body: |
  * **First concept:** A brief explanation.
  * **Second concept:** Building on the first.
    * Supporting detail A (indented with 4 spaces total).
    * Supporting detail B.
---
layout: content
title: Conclusion
body: |
  This is a concluding paragraph.

  * Thank you for your attention.
  * Questions are welcome.
---
```

-----

## Troubleshooting

### "Layout not found" Warning

```
Warning: Layout 'section_header' not found. Using default.
```

This error means the `layout:` value in your markdown (e.g., `section_header`) does not match any layout names in your Google Slides template.

**Solution**:

1.  Run **Cell 4** of the notebook. The `print_layouts(template_id)` function will list all available layout names from your template (e.g., `TITLE`, `TITLE_AND_BODY`, `SECTION_HEADER`).
2.  Ensure the `layout:` values in your Markdown file exactly match one of the names printed by the function (the mapping is case-insensitive).

### Text is in the Wrong Placeholder (e.g., Title appears in Subtitle box)

The script attempts to find placeholders with common names like `TITLE`, `CENTERED_TITLE`, and `BODY`. If your template uses different names for its placeholders, the script may not find them correctly.

**Solution**:

1.  Open your template in Google Slides.
2.  Go to **View \> Master**.
3.  Click on a layout to inspect its placeholders.
4.  This script currently has hard-coded placeholder types it looks for. For more advanced use, you may need to modify the `get_placeholder_ids` function to match the specific placeholder types in your custom template.

README AI generated by Google Gemini 2.5
