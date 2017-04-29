Read more about it [here](https://bhaktipriya96.wordpress.com/gesture-recognition-in-egocentric-videos/)

Gestures are believed to be a more natural and intuitive communication between humans and devices. Ideally, we want users to not think in terms of handling an input device, but naturally use their hands to execute tasks or make use of their skills just as they do for gestural communication with other humans.

We exploit the egocentric paradigm, and focus our attention on hand gestures. Through this method, we hope to help the visually impaired by helping them use simple gestures to interact with their smart phone without physically touching it.  We believe that our Hand gesture recognition framework will make the lives of the users simpler by achieving “hands free” interaction through eliminating the need to hold or press the device.

<strong>Outline of the Gesture Recognition Method:</strong>

<img class=" size-full wp-image-975 aligncenter" src="https://bhaktipriya96.files.wordpress.com/2017/01/screenshot-from-2017-01-20-213505.png" alt="screenshot-from-2017-01-20-213505" width="225" height="402" />

<b>1)Hand Segmentation </b>We have used the methodology implemented in “Pixel-level Hand Detection in Ego-Centric Videos" by Cheng Li and Kris M. Kitani.

This paper extracts  superpixels at each frame using the SLIC algorithm that performs a k-means-based local clustering of pixels in a 5-dimensional space, where color and pixel coordinates are used. Superpixels are represented with several features: histograms in the HSV and LAB color spaces, which have proven to be good representations for the skin. Garbor filters and a simple histogram of gradients  has been utilized to discriminate between objects with a similar color distribution.

In order to make this algorithm robust to various illumination conditions, we also train a collection of Random Forest classifiers indexed by a global HSV histogram. Thus, training images are distributed among the classifiers by a k-means clustering on the feature space. While testing, the predictions from the five nearest classifier are averaged to make the final prediction. To ensure, semantic coherence in time and space, a smoothing filter is applied. This helps in the prediction since each frame is replaced with a combination of the classifier results from past frames. Grab Cut algorithm has been used to exploit spatial consistency. This helps in the removal of small and isolated pixel groups and also to aggregate bigger connected pixel groups.

[gallery ids="507,506,505,504,502,500,496,495" type="slideshow"]

<b>2) Gesture Description using Dense Trajectories</b>

We have used the methodology implemented in “Action Recognition by Dense Trajectories” by Heng Wang et al. We extract feature points around the hands of the user, which we obtain after the hand segmentation step. We use the Improved Dense Trajectories to extract feature points that are densely sampled at several spatial scales and tracked using median filtering in a dense optical flow field. We consider the spatio-temporal volume aligned with each trajectory, and compute the Trajectory descriptor, HOG, HOF and MBH around it.

<b>3)Gesture Recognition & Training</b>

Since the histogram descriptors tend to be sparse, we have applied power normalization to the Dense Trajectory features to obtain our final features. We then create codebooks for each of the descriptors (Trajectory Descriptor, HOG, HOF, MBH) each with a vocabulary size of 500. The final feature vector is then obtained by the concatenation of its four power-normalized histograms. Eventually, gestures are recognized using a linear SVM 1-vs-1 classifier.

<img class="alignnone size-full wp-image-1003" src="https://bhaktipriya96.files.wordpress.com/2017/01/screenshot-from-2017-01-20-215004.png" alt="Screenshot from 2017-01-20 21:50:04.png" width="581" height="221" />

Results:

We test our algorithm on Egocentric Museum Dataset
<ul>
	<li>We sample 2 videos per subject per action, to create a code book (ie.. codebook generated from 10% of the dataset).  Codebook generated by gestures of the Subject under observation.</li>
	<li>Test Train Ratio: 80:20. (1 vs 1 multiclass svm). SVM trained by gestures of only the Subjects under observation.</li>
	<li>We obtain an accuracy of 94% with this approach.</li>
</ul>
https://www.youtube.com/watch?v=2HNOoXSlqXU