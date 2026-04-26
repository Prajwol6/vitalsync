# VitalSync - App Flow

## User: Paramedic

### Step 1: Patient Logging
- Voice input OR quick-tap buttons
- Fields: Name, Age, Injury Time, Symptoms, Allergies, Vitals

### Step 2: AI Protocol Mapping
- Input sent to Claude Sonnet API
- Claude maps symptoms to medical protocols (Ottawa Rules, GCS Scale)
- Returns structured protocol match + risk flags

### Step 3: Crisis Summary Generation
- Claude generates a 5-second readable crisis card
- High contrast UI, large font, color-coded severity

### Step 4: Handover
- Doctor receives the crisis card on their device
- One-tap acknowledgement
