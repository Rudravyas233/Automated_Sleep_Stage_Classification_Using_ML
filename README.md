# Automated_Sleep_Stage_Classification_ML
This project aims to classify various Sleep Stages into five stages: Wake(W), N1 (light sleep), N2 (light sleep), N3 (deep sleep), and REM (rapid eye movement), as defined by the American Academy of Sleep Medicine (AASM). The project uses various Machine Learning and Ensemble learning techniques on the Sleep-edf dataset. The evaluation is done using evaluation metrics such as Confusion Matrix, AUC ROC Curve and other metrics like F1 Score, precision, recall etc. The goal is to achieve atleast 80% accuracy.

## Dataset
For this application purpose, we are using the freely available PSG (Polysomnography) signal sleep recording dataset from Physionet.org (https://physionet.org/content/sleep-edfx/1.0.0/).
The dataset has 2 main types of files:- 
- PSG.edf Files: These are whole-night polysmnographic sleep recordings containing EEG (from Fpz-Cz and Pz-Oz electrode locations), EOG (horizontal), submental chin EMG, and an event marker. The SC*PSG.edf files (see the 'sleep cassette study') often also contain oro-nasal respiration and rectal body temperature.
- Hypnogram File: These files contain annotations of the sleep patterns that correspond to the PSGs. These patterns (hypnograms) consist of sleep stages W, R, 1, 2, 3, 4, M (Movement time) and ? (not scored). All hypnograms were manually scored by well-trained technicians (identified by the eighth letter of the hypnogram filename) according to the 1968 Rechtschaffen and Kales manual [3], but based on Fpz-Cz/Pz-Oz EEGs

## Data Preprocessing
The data processing step is done in multiple steps. For our current application, we are only using EEG Signals and discarding the other from the PSG Signal files. In EEG too we are using the signals procured by  Fpz-Cz and Pz-Oz channels.Data preprocessing is essential for cleaning and transforming raw EEG signals into meaningful features.

- Load EEG Data Files from Directory
  - Purpose: Locate and store paths of all .edf files from the specified directory.
  - Functionality:
      - Defines the destination_directory where EEG files are stored.
      - Iterates through all files in the directory.
      - Filters files ending with .edf (EEG data format).
      - Stores their full paths in the files_with_paths list.
  - Output: A list of .edf file paths, which will be used in further processing.

- Group PSG and Hypnogram File Pairs
  - Purpose: Organize EEG files by subject ID, grouping corresponding PSG and Hypnogram files.
  - Functionality:
      - Extracts the subject ID from the file name (first 15 characters).
      - Identifies file type (PSG for EEG signals, Hypnogram for sleep stage annotations).
      - Groups files into a dictionary with PSG and Hypnogram categories.
      - Returns a list of grouped file pairs for each subject.
  - Output: A structured list containing PSG-Hypnogram pairs for each subject, ensuring proper data alignment for further processing.

- Load Sleep-EDF Dataset
  - Purpose: Load EEG and Hypnogram files from the Sleep-EDF dataset and prepare them for preprocessing.
  - Functionality:
      - Excludes non-EEG channels (EOG, EMG, Temp, Respiration, Event Marker) if load_eeg_only=True.
      - Crops wake periods (default: removes 30 mins of wake signals before and after sleep).
      - Extracts subject ID and recording session number from the filename.
  - Output: Returns a preprocessed raw EEG object (mne.io.Raw) with sleep annotations mapped, ready for further feature extraction.

- Load and Preprocess Multiple EEG Files
  - Purpose: Load all PSG-Hypnogram file pairs and preprocess them for further analysis.
  - Functionality:
      - Iterates through paired PSG and Hypnogram files.
      - Calls load_sleep_physionet_raw() for each pair.
      - Stores the processed EEG raw objects in data_raws.
  - Output: A list (data_raws) containing preprocessed EEG data objects, ready for segmentation and feature extraction.

## EEG Event Markers and Epoch Creation for Sleep Stage Classification
This section of the project details how to generate uniformly spaced event markers from raw EEG annotations and segment the EEG data into fixed-length epochs for further analysis, such as feature extraction and classification.

- **Purpose:**  
  Create uniformly spaced 30-second event markers and segment EEG data into fixed-length epochs.

- **Process:**  
  - **Generate Fixed-Interval Events:**  
    Adjust event timestamps to ensure a 30-second spacing.  
  - **Extract Events from Annotations:**
    Use MNE's `mne.events_from_annotation` to generate all the events in the patient recordings.  
  - **Create Fixed-Length Events:**  
    Align sleep stage markers using a custom `event_maker()` function. generate a new Event list with each event evenly divided into multiple 30 seconds segments.  
  - **Epoch Creation:** 
    Filter valid sleep stage events (IDs 1-5) and segment EEG data into 30-second epochs using `mne.Epochs()`.

- **Output:**  
  - **Epochs List:** EEG data divided into 30-second epochs.  
  - **Labels:** Corresponding sleep stage labels for each epoch.

## Machine Learning and Ensemble Techniques
Sleep stage classification is a **multiclass classification problem** requiring identifying patterns in EEG signals, which are complex and non-linear. The following models are chosen for their effectiveness in **multiclass classification**, and are tuned and optimized to suit the purpose. Their evaluation is done by measuring their performance using standard metrics.

- **Decision Tree**  
  - Works well for structured data and interpretable decision-making.  
  - Handles non-linearity in sleep EEG signals.  

- **Support Vector Machine (SVM)**  
  - Effective for high-dimensional data with clear class boundaries.  
  - The **RBF kernel** helps model non-linearity in EEG data allowing higher dimesional hyperplanes.  

- **Random Forest**  
  - An ensemble of decision trees that reduces overfitting.  
  - Handles imbalanced sleep stage data well.  

- **XGBoost**  
  - A gradient boosting method that improves accuracy.  
  - Regularization reduces overfitting in multiclass classification.  


Best Models Selection: - 
Optimized versions of **Decision Tree, SVM, Random Forest, and XGBoost** were selected based on hyperparameter tuning using:  
- **Grid Search** – Exhaustive search over a defined parameter set.  
- **Randomized Search** – Efficient parameter search over a random subset.  

The best models were then used in **ensemble learning** for improved performance. 

- **Voting Classifier (Hard and Soft Voting)**  
  - Combines predictions from SVM, Random Forest, and XGBoost.  
  - **Hard voting** selects the majority class, while **soft voting** averages class probabilities.

- **Bagging and Boosting Techniques**  
  - **Bagging**: Reduces overfitting by training models on random data subsets.  
  - **Boosting**: Corrects errors by assigning higher weights to misclassified instances.  

- Evaluation Criterion: -
  - **Accuracy**: Measures overall correctness.  
  - **Confusion Matrix**: Visualizes true vs. predicted sleep stages.  
  - **Classification Report**: Provides precision, recall, and F1-score.  
  - **AUC-ROC Curve**: Assesses model performance across different classification thresholds.
 
## Conclusion
- **SVM and XGBoost performed best individually**, capturing complex EEG patterns.  
- **Ensemble models outperformed single models**, especially with **soft voting**.  
- **AUC-ROC analysis confirmed that ensemble techniques improved sleep stage prediction accuracy.**  
