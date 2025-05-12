# Quick SOAP Notes from Doctor Chats: My Cofactor AI Project

Hey there! This project is my take on Cofactor AI's challenge to turn doctor-patient conversations into neat SOAP notes. The big idea? Use Google's Gemini LLM (`gemini-1.5-flash-latest`) to act like a super-fast medical scribe, aiming for notes that look like the "Alexis Rodriguez" example.

## What's in the Box?

*   `transcripts/`: Where your 10 single-visit `.txt` conversations go.
*   `generated_soap_notes/`: Where the AI-written SOAP notes will appear.
*   `soap_generator.py`: The Python script that does the work.
*   `.env` (you'll create this): For your Google API key.
*   `requirements.txt`: Python stuff to install.
*   This `README.md`!

## Get It Running - Quick Start

1.  **Need These:**
    *   Python 3.8+
    *   Your Google AI Studio API Key ([Google AI Studio](https://aistudio.google.com/)).

2.  **Setup (Terminal Time!):**
    *   In the project folder, set up a Python virtual environment (good habit!):
        ```bash
        python3 -m venv .venv 
        source .venv/bin/activate 
        ```
        *(Windows: `.venv\Scripts\activate`)*
    *   Install what's needed:
        ```bash
        pip install -r requirements.txt
        ```
    *   Create a file named `.env` and put your API key in it:
        `GOOGLE_API_KEY=YOUR_KEY_HERE`

## Go Time!

1.  **Transcripts Ready?**
    *   Put your 10 `.txt` transcript files in the `transcripts/` folder.
    *   **Heads Up:** Each file must be *one single doctor's visit*. Split 'em up if needed!

2.  **Run It:**
    In your terminal (with the virtual environment active):
    ```bash
    python soap_generator.py
    ```

3.  **Check the Notes:**
    *   Look in `generated_soap_notes/` for files like `*_SOAP_EnhancedMimic.txt`.
    *   The terminal will show progress.

## How It Works (The Gist)

I'm basically giving the Gemini LLM a super-detailed recipe (the prompt in `soap_generator.py`) to:

*   **Look Like the Example:** Copy the "Alexis Rodriguez" PDF format – headers, paragraph style for S & O, numbered plan.
*   **Facts Only:** Use *only* what's said in the transcript. No guessing!
*   **Handle Blanks:** If info isn't there (like a DOB), write "[Information Not Available in Transcript]".
*   **Sound Clinical (but friendly):** Write the S & O sections like a flowing story, not just bullets.
*   **Patient's Own Words:** Grab important direct quotes from the patient.
*   **Doctor's Details:** Be specific about what exams were done and what the doctor found.
*   **The "Why":** If the doctor explains their thinking, try to include that in the Assessment.
*   **Clear Plan:** List out the plan step-by-step, with reasons if given.

The Python script just feeds the transcript and these instructions to the LLM and saves what comes back.

## Upsides & Downsides

*   **Cool Stuff:**
    *   Automated processing – fast!
    *   Tries hard to match a human-written style.
    *   Good at grabbing facts.
    *   Honest about missing info.

*   **Could Be Better / Keep in Mind:**
    1.  **Transcript is King:** If it's not said in the transcript, the LLM can't write it (especially headers like DOB, Doctor's Name, etc.).
    2.  **Not a Real Doctor:** It's smart, but it doesn't *think* like a doctor or catch super subtle clinical stuff. It only reports stated reasoning.
    3.  **Style Quirks:** It aims for narrative, but `gemini-1.5-flash-latest` can sometimes be a bit more direct. The temperature (`0.99`) is set for creativity, which can sometimes fight very strict formatting – a lower temp (0.2-0.4) might be more consistent for purely factual output if that's preferred.
    4.  **Transcription Errors / LLM Slips:** Bad input from speech-to-text can confuse it. Rarely, any LLM can make a small error.
    5.  **Complex Exams:** Very detailed exams (like full neuro checks) are tough to make *both* narrative *and* perfectly itemized in one go.
    6.  **One File = One Note:** Remember to feed it one distinct patient visit per file!

## Next Steps (If I Had More Time...)

1.  **JSON Output:** Get structured data out, not just text – easier for computers to use.
2.  **Smarter Extraction:** Use traditional code (regex/NER) for really critical bits (like drug doses) *before* the LLM.
3.  **Mini LLM Experts:** A team of LLMs, each a specialist for S, O, A, or P.
4.  **Train It Up (Fine-Tuning):** With lots of good examples, we could make the LLM even better at this.
5.  **Cleaner Transcripts In:** Auto-fix common errors or identify speakers better.
6.  **Human Review Power-Up:** Let doctors review & correct; feed those fixes back to make the AI learn.
7.  **Tweak LLM Settings:** More experiments with lower temperatures for consistency.
8.  **Smarter Exam Notes:** Teach it to recognize specific exam types (like "neuro exam") and use an even more tailored format for those parts.

This was a fun project to explore how LLMs can help with clinical documentation!
