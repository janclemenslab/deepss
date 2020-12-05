# Annotate song

## Initialize or edit song types
You first need to register the different song types you want to annotate. _DeepSS_ discriminates two categories of song types:
- _Events_ are defined by a single time of occurrence. The aforementioned pulse song is a song type of the event category.
- _Segments_ are song types that extend over time and are defined by a start and a stop time. The aforementioned sine song and the syllables of mouse and bird vocalizations fall into the segment category.

:::{figure} xb_make-fig
<img src="/images/xb_make.png" alt="edit annotation types" height="500px">

Create, rename or delete song types for annotation.
:::

If you load audio that has been annotated before, the annotations will be loaded with the audio and can be added, deleted, and renamed via the same _Annotations/Add or edit song types_ menu.


## Create annotations manually

The registered song types can now be activated for annotation using the dropdown menu on the top left of the main window, or via the number keys indicated in the dropdown menu.

:::{figure} xb_create-fig
<img src="/images/xb_create_opt.gif" alt="annotate song" width="700px">

Left clicks in waveform or spectrogram few create annotations.
:::

Song is annotated by left-clicking the waveform or spectrogram view. If an event-like song type is active, a single left click marks the time of an event. A segment-like song type requires two clicks---one for each boundary of the segment.

## Annotate by thresholding the waveform
Annotation of events can be sped up with the "Thresholding mode”, which detects peaks in the sound energy exceeding a threshold. Activate thresholding mode via the _Annotations_ menu. This will display a draggable horizontal line - the detection threshold - and a smooth pink waveform - the energy envelope of the waveform. Adjust the threshold so that only “correct” peaks in the envelope cross the threshold and then press `I` to annotate these peaks as events.

:::{figure} xb_thres-fig
<img src="/images/xb_thres_opt.gif" alt="annotate song" width="700px">

Annotations assisted by thresholding and peak detection.
:::

The threshold mode can be adjusted via _Annotations/Adjust thresholding mode_:
- _Smoothing factor for envelope (seconds)_: Sets the width of the Gaussian used for smoothing the envelope. Too short and the envelope will contain many noise peaks, too long and individual events will become less distinct.
- _Minimal distance between events (seconds)_: If the events typically arrive at a minimal rate, this can be used to reduce spurious detections.

## Edit annotations
Adjust event times and segment bounds by dragging the lines or the boundaries of segments. Drag the shaded area itself to move a segment without changing its duration. Movement can be disabled completely or restricted to the currently selected annotation type via the _Audio_ menu.

Delete annotations of the active song type by right-clicking on the annotation. Annotations of all song types or only the active one in the view can be deleted with `U` and `Y`, respectively, or via the _Annotations_ menu.

:::{figure} xb_edit-fig
<img src="/images/xb_edit_opt.gif" alt="annotate song" width="700px">

Dragging moves, right click deletes annotations.
:::

## Export and save annotations
_DeepSS_ achieves good performance with little manual annotation. Once you have completely annotated the song in the first 18 seconds of the tutorial recording---a couple of pulse trains and sine song segments---you can quickly train a network to help with annotating the rest of the data. Export the data and the annotations for _DeepSS_ - via the `File/Export for DeepSS` to a new folder (not the one containing the original audio):

:::{figure} xb_assemble-fig
<img src="/images/xb_export.png" alt="export audio and annotations" width=500>

Export audio data and annotations
:::

This will open a dialog for fine-tuning the data export:
- _Song types to export_: Select a specific song type to export annotations for. Keep this the default of exporting all annotations. Predicting song in proof-read mode will produce song types ending in `_proposals` - exclude these from the export. Annotations will be saved as a file ending in `_annotations.csv` (see a [description](/technical/data_formats) of the format).
- _Audio file format_: The file format of the exported audio data:
    - _NPZ_: Zipped numpy variables. Will store a `data` variable with the audio and a `samplerate` variable.
    - _WAV_: Wave audio file. More general but also less flexible format. For instance, floating point data is restricted to the range [-1, 1] (see [scipy docs](https://docs.scipy.org/doc/scipy/reference/generated/scipy.io.wavfile.write.html)). Audio may need to be scaled before saving to prevent data loss from clipping.
    - _Recommendation_: Use NPZ---it is robust and portable.
- _Scale factor_: Scale the audio before export. May be required when exporting to WAV, since the WAV format has range restrictions.
- _Start seconds_ & _end seconds_: Export audio and annotations between start and end seconds. Relevant when exporting partially annotated data. For fast training, do not include too much silence at the before the first and after the last annotation to ensure that all parts of the exported audio contain annotated song.

```{note}
To generate a larger and more diverse dataset, annotate and export _multiple recordings into the same folder_. They can then be assembled in a single data set for traing (see next page).
```

```{note}
We also recommend you save the full annotations next to the audio data via the _File/Save annotations_, since the exported annotations only contain the selected range. Saving will create a `csv` file ending in '_annotations.csv', that will be loaded automatically the next time you load the recording.
```

Once data is exported, the next step is to [train the network](/tutorials_gui/train).