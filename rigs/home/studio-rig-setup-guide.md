# Studio Rig Setup Guide

## Quick Start Checklist

### Hardware Connections
- [ ] Bass → Darkglass Anagram input (TS cable) — bypass for dry takes, engage when crafting tones
- [ ] Anagram XLR out → Scarlett Input 1 (XLR cable)
- [ ] Anagram 1/4" out → Trace Elliot ELF input (TS cable) — optional, for analog monitor
- [ ] ELF Speaker Out → 10" Monitor Cab (TS cable) — optional, paired with above
- [ ] Scarlett → Mac Mini (USB-C cable)
- [ ] Scarlett Outputs 1-2 → Adam F5 Monitors (XLR→TS cables)
- [ ] Scarlett Headphone → Audio Technica Headphones (TRS cable)

### Software Setup
- [ ] Install Focusrite Scarlett drivers
- [ ] Install Loopback
- [ ] Install Logic Pro
- [ ] Install Anytune and MoisesAI

## Workflow Configurations

### Workflow 1: Analog Practice Only
**Use Case**: Simple practice without digital components

**Setup**:
1. Connect bass → Anagram input
2. Patch Anagram 1/4" out → Trace Elliot ELF → 10" Monitor Cab
3. Power on Anagram and ELF
4. Adjust volume and EQ on ELF
5. Play through the monitor cab; Anagram XLR out can stay unplugged

**Benefits**: Low latency, immediate response. The Anagram stays in line so the
signal chain matches the digital workflows below — only the monitor path changes.

---

### Workflow 2: Digital Practice with Backing Tracks
**Use Case**: Practice with backing tracks or live streaming

**Setup**:
1. Complete hardware connections
2. Power on all components
3. Launch Loopback
4. Configure "RecordingRouter" device:
   - **Input Sources**: Anytune, MoisesAI, Chrome, Scarlett Ch1
   - **Output Channels**:
     - Ch1-2: Mixed digital sources
     - Ch3-4: Instrument signal (stereo split)
   - **Monitor**: Scarlett Ch1-2 (aggregates Ch1&3, Ch2&4)
5. Set macOS audio output to "RecordingRouter"
6. Launch backing track application
7. Adjust levels in Loopback mixer

**Testing**:
- Verify instrument signal on channels 3-4
- Verify backing track on channels 1-2
- Check monitor mix through studio monitors

---

### Workflow 3: Recording Session
**Use Case**: Professional recording with DAW

**Setup**:
1. Complete Workflow 2 setup
2. Launch Logic Pro
3. Configure Logic Pro audio settings:
   - **Input Device**: RecordingRouter
   - **Output Device**: Scarlett
   - **Sample Rate**: 48kHz (recommended)
   - **Buffer Size**: 64-128 samples (for low latency)
4. Create new project
5. Create tracks:
   - **Bass Track**: Input = RecordingRouter Ch3, Record enabled
   - **Backing Track**: Input = RecordingRouter Ch1, Record enabled
6. Arm tracks for recording
7. Set up monitoring through Loopback

**Recording Process**:
1. Press record in Logic Pro
2. Play along with backing track
3. Monitor through studio monitors/headphones
4. Stop recording when complete
5. Playback and edit as needed

## Software Configuration Details

### Loopback "RecordingRouter" Setup

**Device Configuration**:
- Device Name: "RecordingRouter"
- Channels: 4 input, 4 output

**Input Sources**:
1. **Anytune App**: Route to output channels 1-2
2. **MoisesAI App**: Route to output channels 1-2
3. **Chrome Browser**: Route to output channels 1-2
4. **Scarlett Input 1**: Split to output channels 3-4

**Output Configuration**:
- **Channels 1-2**: Mixed digital sources (backing tracks)
- **Channels 3-4**: Instrument signal (stereo simulation)

**Monitor Setup**:
- Output to Scarlett channels 1-2
- Mix: Ch1+Ch3 → Scarlett Ch1, Ch2+Ch4 → Scarlett Ch2

### Logic Pro Track Configuration

**Bass Track**:
- Input: RecordingRouter channel 3
- Output: Scarlett channels 1-2
- Record enabled
- Monitoring: On

**Backing Track**:
- Input: RecordingRouter channel 1
- Output: Scarlett channels 1-2
- Record enabled
- Monitoring: On

## Backup Chain (Anagram unavailable)

The **HX Stomp** (now primarily on guitar) or a **ToneX One** stands in for the
Anagram. Neither has a balanced XLR out, so add the **Radial JDI Jensen** passive DI:

- [ ] Bass → HX Stomp or ToneX One input (TS cable)
- [ ] HX Stomp / ToneX One output → JDI input (TS cable)
- [ ] JDI DI out → Scarlett Input 1 (XLR cable)
- [ ] JDI thru → Trace Elliot ELF input (TS cable) — optional, for analog monitor

The JDI is passive with no gain stage, so raise Scarlett Input 1 gain ~6-10 dB.

## Troubleshooting

### Common Issues

**No Sound from Instrument**:
1. Check cable connections (bass → Anagram input, Anagram XLR out → Scarlett)
2. Verify the Anagram is powered, not muted, and the preset isn't silent
3. Check Scarlett input gain (Input 1)
4. Verify Loopback routing

**High Latency**:
1. Reduce Logic Pro buffer size
2. Close unnecessary applications
3. Check CPU usage
4. Verify Loopback settings

**Audio Dropouts**:
1. Check USB connection
2. Increase buffer size
3. Close background applications
4. Check for driver updates

**Ground Noise**:
1. Check cable routing
2. Verify power supply isolation
3. Try different USB ports
4. Check for ground loops

### Performance Optimization

**For Recording**:
- Close all unnecessary applications
- Disable WiFi/Bluetooth if not needed
- Use SSD for audio files
- Monitor CPU usage

**For Practice**:
- Lower buffer sizes for minimal latency
- Use direct monitoring when possible
- Keep backing track levels moderate

## Maintenance

### Daily Checks
- [ ] Test all signal paths
- [ ] Verify software configurations
- [ ] Check cable connections
- [ ] Monitor system performance

### Weekly Tasks
- [ ] Clean cable connections
- [ ] Update software if needed
- [ ] Backup configurations
- [ ] Test all workflows

### Monthly Tasks
- [ ] Deep clean equipment
- [ ] Check for driver updates
- [ ] Review and optimize settings
- [ ] Test backup/restore procedures

## Quick Reference

### Volume Levels (Typical)
- **Anagram**: set XLR out level so the Scarlett input peaks without clipping
- **Scarlett Input 1**: -12dB to -6dB (Anagram XLR out); with the backup JDI, add ~6-10 dB of gain since the JDI is passive
- **Scarlett Output**: -10dB to -5dB
- **Studio Monitors**: 9-10 o'clock
- **ELF Input/Master** (only when ELF is patched in as analog monitor): 12 / 10-11 o'clock

### Buffer Settings
- **Practice**: 32-64 samples
- **Recording**: 64-128 samples
- **Mixing**: 256-512 samples

### Sample Rates
- **Practice**: 44.1kHz
- **Recording**: 48kHz
- **High Quality**: 96kHz

### File Formats
- **Recording**: WAV 24-bit
- **Export**: WAV 24-bit or MP3 320kbps
- **Backup**: ZIP archive of project files