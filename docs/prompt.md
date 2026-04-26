Prompt 1 — Initial App Generation
Build a mobile-first web application called VitalSync — an emergency medical handover tool for paramedics.

The app has three screens:

Screen 1: Patient Logging
- A form with these fields: Patient Name, Age, Time of Injury, Chief Complaint, Allergies, Blood Pressure, Heart Rate, Oxygen Saturation
- Two input modes: a voice input button (using Web Speech API) and manual quick-tap fields
- A "Generate Crisis Summary" button at the bottom

Screen 2: AI Analysis (loading state)
- A high-contrast dark screen
- Text: "Analyzing patient data..."
- A subtle pulse animation

Screen 3: Crisis Summary Card
- Large bold font, readable in 5 seconds
- Color-coded severity banner: Red (Critical), Orange (Urgent), Green (Stable)
- Sections: Patient Snapshot, Protocol Match, Risk Flags, Recommended Action
- A "Handover Complete" button at the bottom

Use a dark medical UI theme. Mobile-first. No unnecessary animations.

Prompt 2 — Protocol and Risk Flag Improvement
Improve the Crisis Summary Card:

1. Protocol Match should map to real emergency medical protocols based on the chief complaint. 
Examples: Wrist pain + fall = Ottawa Wrist Rules. Chest pain = ACS Protocol. 
Unresponsive = GCS Scale + ABCDE Assessment. Head injury = NICE Head Injury Guidelines.

2. Risk Flags should also flag abnormal vitals automatically.
Examples: Heart Rate above 100 = Tachycardia flagged. O2 below 94% = Hypoxia flagged. 
BP above 140/90 = Hypertension flagged.

Prompt 3 — Specific Protocol Names
Protocol Match must use specific named medical protocols, not generic labels.
Mapping rules:
- Wrist pain + fall = "Ottawa Wrist Rules"
- Chest pain = "ACS Protocol (MONA)"
- Head injury = "NICE Head Injury Guidelines"
- Unresponsive = "GCS Scale + ABCDE Assessment"
- Shortness of breath = "CPAP/O2 Protocol"
- Stroke symptoms = "FAST Protocol"
- Default fallback = "ABCDE Primary Survey"

Never show "Standard Emergency Assessment" as a protocol name.

Prompt 4 — GCS Breakdown
When "GCS Scale" is matched, provide a detailed breakdown in the summary card for Eye, Verbal, and Motor responses for clearer handover.

Prompt 5 — Triage Logic
Integrate GCS score directly into triage severity classification logic. 
Automatically upgrade patient status to Critical if total GCS score falls below 9.

Prompt 6 — Voice Continuous Listening
Fix voice input to use continuous listening mode.

Current problem: Speech API stops after first pause, only captures partial input.

Fix:
- Enable continuous: true on the SpeechRecognition object
- Keep listening until user taps the voice button again to stop
- Parse the full accumulated transcript at the end, not word by word
- Show a "Listening..." indicator with a stop button so user knows it's still active

Prompt 7 — Voice Field Parsing
Fix voice input field parsing.

Current problem: All spoken text dumps into Chief Complaint instead of correct fields.

Use this exact parsing logic on the final transcript:
- If transcript contains "patient [name]" or starts with a name → populate Patient Name
- If transcript contains "age [number]" or "[number] years" → populate Age
- If transcript contains "heart rate [number]" or "HR [number]" or "pulse [number]" → populate Heart Rate
- If transcript contains "blood pressure [number] over [number]" or "BP [number]/[number]" → populate Blood Pressure
- If transcript contains "oxygen [number]" or "O2 [number]" or "sats [number]" → populate O2 Saturation
- If transcript contains "allergic to [word]" or "allergy [word]" → populate Allergies
- If transcript contains "time [time]" or "injured at [time]" → populate Time of Injury
- Everything else → populate Chief Complaint

Apply all rules simultaneously to the full transcript.
