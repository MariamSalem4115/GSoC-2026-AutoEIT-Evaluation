GSoC 2026 | AutoEIT: Automatic Scoring for Elicited Imitation Tasks
Evaluation Task Submission Candidate: Mariam Salem (Computer Engineering Student)

📌 Project Overview
This repository contains the evaluation pipeline and audited dataset for the AutoEIT project. The goal of this task was to develop an ASR (Automatic Speech Recognition) pipeline to transcribe Spanish Elicited Imitation Task (EIT) audio and perform a rigorous manual audit to ensure data integrity for downstream scoring models.

🛠 Technical Stack
Transcription Engine: OpenAI Whisper (Turbo) - Selected for its high-speed inference and robust multilingual capabilities.

Audio Pre-processing: librosa and noisereduce for spectral gating and amplitude normalization.

Data Engineering: Python (Pandas) for structured transcription mapping and Excel-based auditing.

🔍 Data Audit & Quality Control
During the evaluation of the four participants, I identified several edge cases where standard ASR models fail. I manually corrected these to provide a "Ground Truth" level dataset:

1. Phonetic Hallucination Loops (The "Perro" Effect)
Issue: High-confidence phonetic features, specifically the Spanish trilled /r/ in words like perro, triggered "decoding shocks" in the model, leading to repetitive loops (12+ cells of identical output).

Fix: Manually deduplicated and aligned segments to match the target 30-sentence stimuli.

2. Signal-to-Noise Ratio (SNR) Challenges
Issue: Participant 3 exhibited low vocal amplitude and slow speech rates, causing the ASR to "hallucinate" nonsense sentences during silent intervals.

Fix: Implemented a terminal silence cutoff. Segments falling below the usable threshold were marked as [unintelligible] to maintain scientific honesty over hallucinated data.

3. Speaker Diarization
Issue: Raw ASR output captured both the male instructor's prompts and the student's responses.

Fix: Isolated the student-specific utterances to ensure the final report reflects only the participant's performance.

🚀 Proposed GSoC Technical Roadmap
To solve these issues in the full implementation, I propose:

Voice Activity Detection (VAD): Using WebRTCVAD to "tighten" the audio segments and eliminate silence-based hallucinations.

Dynamic Range Compression: A pre-processing step to boost low-volume speakers before they hit the ASR engine.

Fine-tuned Decoding: Adjusting Temperature and Logprob thresholds in Whisper to prevent repetitive loops on complex phonemes.

📂 Repository Structure
HumanAI_1st_try.ipynb: The complete Python pipeline from raw audio to transcription.

AutoEIT_Final_Audited_Transcriptions.xlsx: The 100% audited and corrected "Ground Truth" data.

AutoEIT_Report.pdf: A static summary of the findings and processing logs.