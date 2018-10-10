# Applying Surrogate-supervision on Diabetic Retonopathy Classification

## Data and Background
Diabetic Retinopathy (DR) is an eye disease associated with long-standing diabetes. The different stages of DR can be determined by the severity and position of its associated lesions that arise from vascular abnormalities. The data obtained from the Kaggle Dibetic Retinopathy Detection [1] challenge consists of a total of nearly 88,000 color fundus images that each have one of 5 labels based on the level of DR associated with that image- 0: No Diabetic Retinopathy, 1: Mild  Diabetic Retinopathy(microaneurysms),  2: Moderate  Diabetic Retinopathy(mild hemorrhage/exudates),  3: Severe  Diabetic Retinopathy(severe hemorrhage, exudates and/or venous beading),  4: Proliferative  Diabetic Retinopathy(neovascularization and/or pre-retinal hemorrhage). The data is split into train (70,000 images), validation (3,000) and test (15,000) sets for our experiments.

## Self supervised training and architecture
We used an Inception-resnet-v2 to train our model that distinguishes the 5 classes of DR. To pre-train this architecture, we used rotation as surrogate supervision by appending a fully connected layer with 3 neurons corresponding to 0°, 90°, and 270° rotation followed by softmax with a cross entropy loss layer. Note that applying a 180° rotation on the retina image from the left eye results in an image with appearance to the unrotated retina image from the right eye. In view of this ambiguity, we did not use a 180° rotation during training with surrogate supervision. Nevertheless, rotation is a reasonable surrogate for DR because fundus images have a consistent geometry; thus, our model can learn the relative locations of structures such as the macula, optic disc, and retinal network by learning to predict the rotation. We trained the network for 20000 epochs using an adam optimizer with an initial learning rate of 10-4  a learning rate decay factor of 0.8 after the first 40 epochs. The cost function used was a focal loss with parameters α=0.6 and γ=2.

## Supervised training 
The supervised multi-class DR classification model was also trained using an Inception-resnet-v2. The model was trained to distinguish the 5 classes from scratch for 250 epochs (500,000 steps) using a batch size of 8 with an initial learning rate of 10-3 followed by a manual learning rate decrease every 100 epochs. We used a momentum optimizer with a momentum of 0.9, and a softmax cross-entropy loss function. 

## References
[1]
