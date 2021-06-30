# TcGraph Coding Conventions

#### Code
```declaration
// Running on a 1ms Task
PROGRAM MainCM
VAR
    el3632 : EL3632_2xCh_10xOversampleFactor;
	analogRead : HS_AnalogRead(
		AnalogData := el3632,
		AnalogScaling := Scaling.none,
		SampleRate := SampleRates._10kHz
	);
END_VAR
```

```body
IF NOT analogRead.IsInitialised THEN
    analogRead.Initialise();
END_IF

analogRead.Read();
```

```declaration
// Running on a 10ms Task
PROGRAM MAIN
VAR
	FFT : HS_FFT_Real(
		Source := analogRead,
		MinimumFrequencyStep := 1
	);

	// 10khz, 1hz per index (actual frequency step 0.610352)
	resultsArray : ARRAY [0..8192] OF LREAL;

END_VAR
```

```body
FFT.CyclicProcess();

IF FFT.HasResult THEN
	FFT.CopyResultsToArray(resultsArray);
END_IF

```

## Library Components

### Analog Processors

#### HS_FFT
```code
	FFT : HS_FFT_Real(
        Source := hsAnalogRead,
        MinimumFrequencyStep := 1.0
    );
```