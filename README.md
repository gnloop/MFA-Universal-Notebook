### [Montreal Forced Aligner](https://montreal-forced-aligner.readthedocs.io/) Universal Notebook
___
#### Huge thanks to *PixPrucer* and *HAI-D* for making the original notebook and script, I just updated it to the latest MFA version and added a menu to choose any compatible language.
___

*Remember to cut up your samples in smaller bits before uploading them OR use the inbuilt slicer, MFA hates long samples!*

**Please refer to this:** <br>
https://github.com/openai/whisper <br>
https://mfa-models.readthedocs.io/en/latest/ <br>
**To check if this notebook will work for your language.**

## How to use

### MFA Universal Notebook: <a href="https://colab.research.google.com/github/gnloop/MFA-Universal-Notebook/blob/slicer/MFA_Notebook.ipynb"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab" style="width: 120px;"/> </a>

*Before proceeding, it is highly recommended you use Google Drive to store your files.*

**1. Preparing Your Audio Files**

Upload your dataset to Drive in a .zip file containing nothing but your .wav files:
<pre>
# EXAMPLE:
wav.zip:
    |
    |
    audio_1.wav
    audio_2.wav
    audio_3.wav
    ...
</pre>

Follow the steps in the notebook, making sure to point it to the location of your zipped audio files (e.g., `/content/drive/MyDrive/YourZipFile.zip`). **If your audio samples are longer than about 30 seconds each, tick the ***'slice_samples'*** checkbox**. Otherwise, leave it unchecked.

**2. Handling Transcriptions**

**If you already have text transcriptions for your dataset,** you can skip the Whisper steps! Just upload your transcriptions as a zip file to Google Drive and run the ***"(Optional) Unzip edited transcriptions"*** cell in the notebook.

If you don't have transcriptions, don't worry! Whisper can automatically generate them for you. Install it and run the ***'Whisper inference'*** cell. Keep in mind that Whisper's transcriptions aren't always perfect, so it's a good idea to review and edit them before moving on to the alignment process.

**3. Aligning with MFA**

For the MFA steps, carefully follow the instructions in the notebook. You'll need to choose the appropriate acoustic and dictionary models for your language from the MFA website.

To find the model names:

1. Go to the [MFA Website](https://mfa-models.readthedocs.io/en/latest/) and select your desired acoustic model/dictionary language.
2. Scroll down on the model/dictionary page to the 'Installation' section.

For instance, as shown in the image below, you'd use 'spanish\_mfa' for the Spanish acoustic model:

![image](https://github.com/user-attachments/assets/1a5ebdfa-6907-4ed7-be21-71d54318a08d)

**3.5. (Optional) Generating a custom dictionary with G2P Models**

If the dictionary provided by MFA does not have certain words (common in languages such as Japanese and Spanish), you can generate a custom dictionary from a G2P model!

For instance, as shown in the image below, you'd use 'spanish\_spain\_mfa' for the Spanish (with the phonemic difference of Spain) G2P model:

![image](https://github.com/user-attachments/assets/7c76ed8b-25a9-4ca5-8305-746a2fff51a4)

**4. Converting TextGrid to HTK Labels**

Next, you'll need to convert the TextGrid files (which MFA outputs) into monophonic HTK `.lab` files that DiffSinger can use. Follow the steps in the notebook, but be mindful of the converter you choose.

If your language is already in the dropdown menu, you're good to go! If not, you'll need to create a custom converter. This is because most MFA models use a custom form of IPA (International Phonetic Alphabet) in their alignment, which isn't compatible with DiffSinger.

**Creating a Custom Converter:**

1. Refer to the MFA page for your chosen dictionary model to get a list of all the phonemes it uses.
2. Create a .txt file named `custom_converter.txt`.
3. Structure the file as follows:

    ```
    PhonemeToBeReplaced,ReplacementPhoneme
    ```

    For example, to convert all characters to lowercase, your file would look like this:

    ```
    A,a
    B,b
    C,c
    ...and so on
    ```
     
    You can see some examples of converters [here](https://github.com/gnloop/MFA-Universal-Notebook/tree/slicer/converters).

**5. (Optional) Italian-Specific Step**

There's an extra step specifically for Italian datasets to enhance compatibility. If you're working with Italian, follow this step. Otherwise, feel free to skip to the ***'Zip output'*** step. If you used the ***'slice_samples'*** option earlier, remember to also check the ***'save_samples'*** option here.

**6. Saving Your Output**

Choose your preferred output path (I recommend something like `/content/drive/MyDrive/MFA_output`). **Make sure there's no '/' at the end of the path!** This is where your zip files containing your processed samples and labels will be saved.

**Note:** Currently, there isn't an option to save TextGrid labels directly. This guide focuses on generating HTK labels for DiffSinger. However, you're welcome to adapt it to save TextGrid labels if needed.

## UTAU Converters (Experimental)

I've included [some experimental notebooks](https://github.com/gnloop/MFA-Universal-Notebook/tree/slicer/utau_conversion) to convert UTAU banks into datasets usable by DiffSinger. So far, I've only had good results with the Arpasing converter, but feel free to experiment.

## Known Issues

Here are a few things to keep in mind:

*   It really struggles with long silences, long notes and humming.
*   It's really dependent on Whisper's performance as well, which isn't always perfect: if you want a better base it's highly recommended to edit the transcriptions!
*   The pretrained dictionaries for MFA are often lackluster and/or inaccurate when transposed to singing, which affects the label quality (I've particularly noticed this with French).

## Advantages Over SOFA

*   **Wider Language Support:** MFA supports a much broader range of languages compared to SOFA.
*   **More Precise Alignment:** MFA generally provides more accurate alignments than SOFA. While SOFA makes educated guesses, MFA is very precise when the issues mentioned above aren't present.
*   **Better with Speech:** Since MFA excels at speech processing, it works exceptionally well for spoken or rapped vocals.
*   **Potential for Custom Model Training:** If you have a large enough dataset, you could even train your own MFA model without needing any labeled data! This is beyond the scope of this guide, but I might create a training notebook in the future if there's enough interest.

---
