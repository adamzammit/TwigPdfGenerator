# TwigPdfGenerator

A LimeSurvey plugin that uses Twig to generate a PDF report based on response data. The PDF report can be automatically emailed to the respondent on completion. Enable in the Simple plugins section of a survey. Can be used to generate a report that includes assessments particularly using multiple choice questions.

## Documentation

### Installation
1. Download the plugin and extract to the plugins directory (or upload ZIP file)
2. Enable the plugin

### Usage
1. Go to "Simple plugin settings" for your survey
2. Decide if you want the report automatically emailed or not (it will look for the question with code "email", and if that doesn't exist, the email from the participant table to extract the email address from).
3. Enter in the PDF metadata details (Title, Author, Header, Subject etc)
4. Enter in the PDF filename (you can use twig to customise this, but please make sure it ends in ".pdf")
5. Enter in the Mailing subject line (again you can use twig)
6. Enter in the PDF and Mailing template using twig
    - Multiple choice questions with a subquestion code starting with "C" will be treated as "correct".
        - A twig subitem for each question will be generated containing "truePositives" if the question was selected
        - A twig subitem for each question will be generated containing "falseNegatives" if the question was selected
    - Multiple choice questions with a subquestion code starting with "I" will be treated as "incorrect".
        - A twig subitem for each question will be generated containing "falsePositives" if the question was selected
        - A twig subitem for each question will be generated containing "trueNegatives" if the question was selected
    - Additional subitems are created for:
        - group_id - the LimeSurvey gid
        - code - The LimeSurvey question code
        - text - The question text in the current language
        - helpText - The help text in the current language
        - value - The assessment score/value
    - Twig "response" contains all response data (eg {{ response.Q01 }} will be replaced with the data entered in question Q01
    - Twig "questions" contains all questions with subitems as above (see example below)


Some example twig template code:


```
<h1>Questions for {{ response.firstname }} {{ response.lastname }}</h2>
{% for question in questions %}
 {% set qnumber = question.code[1:2] %}
 <strong>Question {{qnumber}}:</strong> {{ question.text }}<br>
{% endfor %}
```


7. When viewing responses, the action menu (...) will now have a new entry "Download custom report" which will download the report for that response

## Copyright
- Copyright 2025 The Australian Consortium for Social and Political Research Incorporated (ACSPRI) <https://www.acspri.org.au>
- Licence : GNU General Public License <https://www.gnu.org/licenses/gpl-3.0.html>
- Author: Adam Zammit: <adam.zammit@acspri.org.au>
- Original author: Sam <sam@mousa.nl> / https://gitlab.com/valuematch/twig-pdf-generator 
