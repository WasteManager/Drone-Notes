# ================================
# Betaflight CLI Rate Presets (5")
# Uses RATEPROFILE 0-3
# Paste ALL of this into CLI.
# If your BF version doesn't support 'rates_type', it will error—safe to ignore.
# ================================

# Force Betaflight rates type (ignore if unsupported)
set rates_type = BETAFLIGHT


# ------------------------------------------------
# RATEPROFILE 0: Easy / Smooth / Forgiving (Mode 2 transition)
# Roll:  RC 0.70 | SR 0.65 | Expo 0.30
# Pitch: RC 0.70 | SR 0.65 | Expo 0.30
# Yaw:   RC 0.60 | SR 0.55 | Expo 0.25
# ------------------------------------------------
rateprofile 0
set roll_rc_rate = 70
set pitch_rc_rate = 70
set yaw_rc_rate = 60
set roll_srate = 65
set pitch_srate = 65
set yaw_srate = 55
set roll_expo = 30
set pitch_expo = 30
set yaw_expo = 25


# ------------------------------------------------
# RATEPROFILE 1: Indoor / Tight-space Control (for a 5")
# Roll:  RC 0.55 | SR 0.45 | Expo 0.45
# Pitch: RC 0.55 | SR 0.45 | Expo 0.45
# Yaw:   RC 0.50 | SR 0.40 | Expo 0.40
# ------------------------------------------------
rateprofile 1
set roll_rc_rate = 55
set pitch_rc_rate = 55
set yaw_rc_rate = 50
set roll_srate = 45
set pitch_srate = 45
set yaw_srate = 40
set roll_expo = 45
set pitch_expo = 45
set yaw_expo = 40


# ------------------------------------------------
# RATEPROFILE 2: Freestyle (snappy but controllable)
# Roll:  RC 0.85 | SR 0.75 | Expo 0.20
# Pitch: RC 0.85 | SR 0.75 | Expo 0.20
# Yaw:   RC 0.75 | SR 0.70 | Expo 0.15
# ------------------------------------------------
rateprofile 2
set roll_rc_rate = 85
set pitch_rc_rate = 85
set yaw_rc_rate = 75
set roll_srate = 75
set pitch_srate = 75
set yaw_srate = 70
set roll_expo = 20
set pitch_expo = 20
set yaw_expo = 15


# ------------------------------------------------
# RATEPROFILE 3: Long Range / Cinematic Cruising
# Roll:  RC 0.65 | SR 0.55 | Expo 0.35
# Pitch: RC 0.65 | SR 0.55 | Expo 0.35
# Yaw:   RC 0.60 | SR 0.50 | Expo 0.30
# ------------------------------------------------
rateprofile 3
set roll_rc_rate = 65
set pitch_rc_rate = 65
set yaw_rc_rate = 60
set roll_srate = 55
set pitch_srate = 55
set yaw_srate = 50
set roll_expo = 35
set pitch_expo = 35
set yaw_expo = 30


# Save to EEPROM
save
