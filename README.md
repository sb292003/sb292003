import PyPDF2
import re

def extract_text_from_pdf(pdf_path):
    try:
        with open(pdf_path, 'rb') as file:
            reader = PyPDF2.PdfReader(file)
            text = ""
            for page in reader.pages:
                text += page.extract_text()
            return text
    except FileNotFoundError:
        return "Error: PDF file not found."
    except Exception as e:
        return f"An error occurred: {e}"

def answer_question(text, question):
    # Very basic keyword matching (highly simplified)
    keywords = re.findall(r'\b\w+\b', question.lower()) # Extract words
    matches = []
    for keyword in keywords:
        if keyword.lower() in text.lower():
            matches.append(keyword)

    if matches:
        # Find the sentence containing the most matches
        sentences = re.split(r'(?<!\w\.\w.)(?<![A-Z][a-z]\.)(?<=\.|\?)\s', text) #split into sentences
        best_sentence = ""
        max_matches = 0
        for sentence in sentences:
          match_count = sum(1 for match in matches if match.lower() in sentence.lower())
          if match_count > max_matches:
            max_matches = match_count
            best_sentence = sentence
        return best_sentence
    else:
        return "I couldn't find an answer based on keywords."


# Example usage (you'll need a PDF file named "paper.pdf")
pdf_file = "paper.pdf"
extracted_text = extract_text_from_pdf(pdf_file)

if "Error" not in extracted_text:
    question = input("Ask a question about the paper: ")
    answer = answer_question(extracted_text, question)
    print("Answer:", answer)
else:
    print(extracted_text)
    

<!---
sb292003/sb292003 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
