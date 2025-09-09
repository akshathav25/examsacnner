# Full-Stack Document Scanner Assignment

### Current Status & Known Issues

* **Frontend:** The React frontend is **100% complete and fully functional locally**. [cite_start]All required features, including user authentication [cite: 15][cite_start], file upload for PNG/JPEG/PDF [cite: 16][cite_start], a gallery view for logged-in users [cite: 21][cite_start], and a before/after detail view with zoom/pan[cite: 18], are implemented.
* [cite_start]**Backend:** The Python Cloud Function for the auto-crop logic is **100% written**[cite: 17].
* **Known Issue:** The project is currently facing a persistent deployment issue with the Python Cloud Function on Google Cloud Build. The CLI incorrectly reports a successful deployment, but the function does not appear in the console. Due to the deadline, this issue could not be resolved in time.
* **Result:** Because the backend function is not live, the **publicly deployed app is not fully functional**. Uploads will result in an error. To test the complete application, the code must be run locally using the setup instructions below.

---

**Live App URL:** [Your Firebase Hosting URL Here - e.g., https://doc-scanner-final.web.app]
**Test Credentials:** Please see "Setup Instructions" to run locally and test, due to the backend deployment issue.

---

### [cite_start]Architecture Overview and Data Flow [cite: 42]

This application uses a React single-page app for the frontend and Firebase for its backend services. The data flow is as follows:
1.  A user signs up or logs in using Firebase Authentication.
2.  The user selects an image or PDF from the React UI and uploads it to Firebase Storage.
3.  A corresponding document is created in the Firestore 'scans' collection with metadata and an 'uploading' status.
4.  A Python Cloud Function is triggered by the new file in Storage.
5.  The function processes the image using OpenCV, corrects the perspective, and saves the new image back to Storage.
6.  The function updates the Firestore document with the new URL and 'completed' status.
7.  The React app's real-time listener updates the UI to show the completed status.

### [cite_start]How Auto-Crop Works (Algorithm Steps) [cite: 43]

The perspective correction is handled by a Python Cloud Function using the OpenCV library. The algorithm follows these steps:
1.  The image is converted to grayscale.
2.  A Gaussian blur is applied to reduce noise.
3.  Canny edge detection is run to find edges.
4.  `cv2.findContours` is used to find all shapes.
5.  The contours are sorted by area, and the algorithm iterates through them to find the first contour with four corners.
6.  A perspective transform (`cv2.warpPerspective`) is applied using the four corners to get the final top-down, rectangular image.

### [cite_start]Libraries Used [cite: 44]

* **Frontend:** React (MIT), React Router DOM (MIT), Firebase JS SDK (Apache 2.0), pdf.js (Apache 2.0), react-zoom-pan-pinch (MIT)
* **Backend:** OpenCV-Python (MIT), NumPy (BSD), Firebase Admin SDK (Python) (Apache 2.0)

### [cite_start]Setup Instructions [cite: 45]

1.  Clone the repository.
2.  Create a Firebase project and enable Authentication, Firestore, and Storage.
3.  Create a web app in your Firebase project and copy the configuration object into a new `client/src/firebase.js` file.
4.  In the `client` directory, run `npm install`.
5.  In the `functions` directory, create a Python virtual environment and run `pip install -r requirements.txt`.
6.  To test the full application, use the Firebase Emulators to run the Cloud Function locally while running the React app with `npm start`.

### [cite_start]Trade-offs & What You'd Improve Next [cite: 46]

* **Trade-off:** The image processing is handled entirely on the backend for robustness. A future improvement would be a client-side preview of the detected corners before uploading to give the user more control.
* **Next Step:** The immediate next step would be to resolve the Google Cloud Build deployment issue by investigating the detailed build logs and IAM permissions for the Cloud Build service account.