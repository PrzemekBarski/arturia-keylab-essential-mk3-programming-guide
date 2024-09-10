# Arturia Keylab Essential MK3 programming guide

Welcome hackers!

This is an unofficial guide to programming the Keylab Essential MK3 keyboards under DAW mode. I was able to put it together by reverse engineering the Logic Pro and FL Studio DAW integration scripts. I hope you'll find it useful.

## General

Message format:

    F0 00 20 6B 7F 42 (DATA) F7

DAW connection:

    02 0F 40 5A 01

DAW disconnection:

    02 0F 40 5A 00

## Buttons

RGB Button:

    04 01 16 (button_id) (r) (g) (b)
    
    //  button_id:
    //
    //    MIDI CH:     00
    //    Pad Bank:    01
    //    Transp-:     02
    //    Transp+:     03
    //    Oct-:        04
    //    Oct+:        05
    //
    //    Pads 1-16:   1C - 2B
    //
    //    Save:        0C
    //    Quantize:    0D
    //    Undo:        0E
    //    Redo:        0F
    //
    //    Loop:        10
    //    Rewind:      11
    //    FF:          12
    //    Metronome    13
    //    Stop:        14
    //    Play:        15
    //    Record:      16
    //    Tap Tempo:   17
    //
    //    Context 1-4: 18 - 21
    //
    //    Prog:        06
    //    Part:        07
    //    Arp:         08
    //    Chord:       09
    //    Scale:       0A
    //    Hold:        0B
    // 
    //
    //  r,g,b: 00 - 7F

## Presets

Switching keyboard presets:

    21 11 40 02 00 (preset_index)
    
    //  preset_index: 00 - 07


## Screen

To display something on the screen you need to:

1. Set one of the screen types (10 - 1A) and it's contents
2. (Optional) Set header and/or footer. Only some of the screen types allow headers and footers.

### Screen types

Theoretically, `line_1` and `line_2` will always accept 18 characters, but you shouldn't use more than 12 on K, F and P screen types. TI2L will fit 12 characters on `line_1` and 18 characters on `line_2`. P2L will fit 14 characters on `line_1` and 18 on `line_2`. If you use both Header and Footer on screen type TI2L, `line_1` and `line_2` will be squished. If you use exceed 18 characters on any line, the text will be trimmed and ellipsis will be added after 18th character. I don't know what is the difference between F1l and 1L, or 2L and 2LS, they look the same to me.

    //  line_1: ASCII string (18 characters)
    //  line_2: ASCII string (18 characters)
    //  transient: 00 (screen is permanent) or 01 (screen is temporary)
    //  hw_value: 00 - 7F  (knob, fader or pad value)
    //  icon: 00 - 4B (there is alist of available icons later in this document)

F1L (1 line of text centeered, Header and Footer allowed):

    04 01 60 10 01 (line_1) 00 (transient)

1L  (1 line of text centered, Header and Footer allowed):

    04 01 60 11 01 (line_1) 00 (transient)

2L  (2 lines of text centered, Header and Footer allowed):

    04 01 60 12 01 (line_1) 00 02 (line_2) 00 (transient)

2LS (2 lines of text centered, Header and Footer allowed):

    04 01 60 13 01 (line_1) 00 02 (line_2) 00

K   (Knob on the left, 2 lines of text on the right):

    04 01 60 14 01 (line_1) 00 02 (line_2) 00 03 (hw_value) 00 (transient)

F   (Fader on the left, 2 lines of text on the right):

    04 01 60 15 01 (line_1) 00 02 (line_2) 00 03 (hw_value) 00 (transient)

P   (Pad on the left, 2 lines of text on the right):

    04 01 60 16 01 (line_1) 00 02 (line_2) 00 03 (hw_value) 00 (transient)

P2L (Pop-up message, 2 lines of text centered:

    04 01 60 17 01 (line_1) 00 02 (line_2) 00 01

B2L (2 lines of text centered and blinking, Header and Footer allowed):

    04 01 60 18 01 (line_1) 00 02 (line_2) 00 (transient)

LI2L (Icon on the left, 2 lines of text on the right, Header and Footer allowed):

    04 01 60 19 01 (line_1) 00 02 (line_2) 00 03 (icon) 00 (transient)

TI2L (Icon on the top, 2 lines of text on the bottom, Header OR Footer allowed):

    04 01 60 1A 01 (line_1) 00 02 (line_2) 00 03 (icon) 00 (transient)

Clear screen:

    04 01 60 61

### Header and Footer

Header:

    04 01 60 01 02 (line_1) 00 00
    
    //  line_1: ASCII string (18 characters)

Footer:

    04 01 60 03 (Button1) (Button2) (Button3) (Button4)
    
    //  Button creation is explained below


// X: button type (0: state, 1: text, 2: icon)
// data: state (?) | text (ASCII string) | icon (00 - 4B)
//
// example: 22 03 00 (Button 2 with Bass icon)

Buttons:

    Button1:   1X (data) 00
    Button2:   2X (data) 00
    Button3:   3X (data) 00
    Button4:   4X (data) 00
    
    //  X: button type (0: state, 1: text, 2: icon)
    //  data: state (?) | text (ASCII string) | icon (00 - 4B, list below)
    //
    //  example: 22 03 00 (Button 2 with Bass icon)


### List of icons

    None:           00
    Logo:           01
    Back:           02
    Bass:           03
    BassMini:       04
    Brass:          05
    BrassMini:      06
    FullOctave:     07
    Computer:       08
    SmallPot:       09
    SmallDelete:    0A
    Drums:          0B
    DrumsMini:      0C
    EPiano:         0D
    EPianoMini:     0E
    FW:             0F
    KeyType1:       10
    KeyType2:       11
    KeyType3:       12
    KeyType4:       13
    KeyType1Full:   14
    KeyType2Full:   15
    KeyType3Full:   16
    KeyType4Full:   17
    Keys:           18
    KeysMini:       19
    Lead:           1A
    LeadMini:       1B
    Like:           1C
    EmptyLike:      1D
    MidiDin:        1E
    Organ:          1F
    OrganMini:      20
    Pads:           21
    PadsMini:       22
    LeftArrow:      23
    RightArrow:     24
    LeftArrowFull:  25
    RightArrowFull: 26
    Pencil:         27
    Piano:          28
    PianoMini:      29
    Replacing:      2A
    Reset:          2B
    ResetMini:      2C
    Sequence:       2D
    SequenceMini:   2E
    SFX:            2F
    SFXMini:        30
    Strings:        31
    StringsMini:    32
    Plus:           33
    Update:         34
    User:           35
    UserMini:       36
    Vocal:          37
    VocalMini:      38
    BottomBracket:  39
    TopBracket:     3A
    LeftBracket:    3B
    RightBracket:   3C
    Selected:       3D
    Bank:           3E
    Mixer:          3F
    Search:         40
    BigOctave:      41
    Arm:            42
    LiveS:          43
    LiveA:          44
    Bitwig:         45
    FL:             46
    Live:           47
    Reason:         48
    Cubase:         49
    Logic:          4A
    LastEntry:      4B

## Example messages

Before you start you need to initialize the DAW connection (you don't need the actual DAW, it's just the type of connection that allows for SysEx control)

    F0 00 20 6B 7F 42 02 0F 40 5A 01 F7

Now that you're connected, you can try and set Pad 1 colour to blue:

    F0 00 20 6B 7F 42 04 01 16 1C 00 00 7F F7

### LCD

#### Screen types

F1L:

    F0 00 20 6B 7F 42 04 01 60 10 01 4C 69 6E 65 20 31 00 00 F7

1L:

    F0 00 20 6B 7F 42 04 01 60 11 01 4C 69 6E 65 20 31 00 00 F7

2L:

    F0 00 20 6B 7F 42 04 01 60 12 01 4C 69 6E 65 20 31 00 02 35 30 25 00 00 F7

2LS:

    F0 00 20 6B 7F 42 04 01 60 13 01 4C 69 6E 65 20 31 00 02 35 30 25 00 F7

K:

    F0 00 20 6B 7F 42 04 01 60 14 01 4C 69 6E 65 20 31 00 02 35 30 25 00 03 40 00 00 F7

F:

    F0 00 20 6B 7F 42 04 01 60 15 01 4C 69 6E 65 20 31 00 02 35 30 25 00 03 40 00 00 F7

P:

    F0 00 20 6B 7F 42 04 01 60 16 01 4C 69 6E 65 20 31 00 02 35 30 25 00 03 40 00 00 F7

P2L:

    F0 00 20 6B 7F 42 04 01 60 17 01 4C 69 6E 65 20 31 00 02 35 30 25 00 01 F7

B2L:

    F0 00 20 6B 7F 42 04 01 60 18 01 4C 69 6E 65 20 31 00 02 35 30 25 00 00 F7
LI2L:

    F0 00 20 6B 7F 42 04 01 60 19 01 4C 69 6E 65 20 31 00 02 35 30 25 00 03 03 00 00 F7

TI2L:

    F0 00 20 6B 7F 42 04 01 60 1A 01 4C 69 6E 65 20 31 00 02 35 30 25 00 03 03 00 00 F7

#### Header and Footer

Header:

    F0 00 20 6B 7F 42 04 01 60 01 02 4C 69 6E 65 20 31 00 00 F7

Footer:

    00 20 6B 7F 42 04 01 60 03 11 42 74 6e 31 00 20 02 00 31 42 74 6e 33 00 42 18 00
