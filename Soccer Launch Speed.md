---


---

<h1 id="soccer--launch-velocity-algorithm">Soccer  Launch Velocity Algorithm</h1>
<p>This algorithm is designed to estimate the <strong>launch angle</strong> and <strong>initial speed</strong> of a soccer ball based on UWB (Ultra-Wideband) tracking data and IMU derived ball flight features. This algorithm takes a dictionary of flight features: (<code>duration</code>, <code>speed</code>, and <code>spin</code>), and the arrays of the ball’s x and y pitch positions and UWB timestamps to compute an estimated launch angle and associated initial velocity.</p>
<p>The process involves:</p>
<ol>
<li><strong>Angle Approximation</strong>: Predicting the ball’s launch angle using pre-trained supervised machine learning regression models.</li>
<li><strong>Speed Estimation</strong>: Computing the initial velocity using trajectory fitting or linear regression-based fallback methods if the flight duration is less than 0.5 seconds.</li>
</ol>
<hr>
<h3 id="angle-approximation">Angle Approximation</h3>
<p>Random Forest and XGBoost PKL files are located within the src/ball module of the lib–algorithms, and are required to load in the regression-based models and subsequently provide an estimate of the launch angle of the ball for each event.</p>
<p><strong>Inputs</strong>:</p>
<ul>
<li>Event features for each flight: <code>duration</code>, <code>speed</code>, and <code>spin</code> are extracted from the ball IMU data. In this case saved as eventData.csv</li>
</ul>
<p><strong>Procedure</strong>:</p>
<ul>
<li>
<p>Two pre-trained machine learning models have been prepared using the  <strong>Random Forest (RF)</strong> and  <strong>XGBoost (XGB)</strong> model architectures, which are both ensemble learning methods and use aggregated decision trees to optimise the performance.</p>
</li>
<li>
<p>However XGBoost requires considerably more data for accurate estimation, and as of yet the random forest model is more robust against overfitting with sparse data and has been used thus far.</p>
</li>
<li>
<p>Event feature dictionaries are passed through the RF and XGB using model.predict function to give an estimate for the launch angle.</p>
</li>
</ul>
<p><strong>Outputs</strong>: Predicted angles for each event are added to the dataframe, ready for use in subsequent speed estimation.</p>
<hr>
<h3 id="speed-estimation">Speed Estimation</h3>
<ul>
<li><strong>Inputs</strong>:
<ul>
<li>UWB tracking arrays:
<ul>
<li><code>tUWB</code>: Timestamps.</li>
<li><code>posX</code>: X-coordinates of the ball’s position.</li>
<li><code>posY</code>: Y-coordinates of the ball’s position.</li>
</ul>
</li>
<li>Predicted angles (<code>RF_PredictedAngle</code> or <code>XGB_PredictedAngle</code>).</li>
<li>Event timestamps (<code>startTime</code>, <code>endTime</code>).</li>
</ul>
</li>
<li><strong>Procedure</strong>:
<ul>
<li>Extract the relevant UWB data within the event time window.</li>
<li>Use a <strong>2nd-order polynomial fit</strong> to calculate speed components based on the trajectory.</li>
<li>If polynomial fitting fails (e.g., insufficient data or flight duration too low):
<ul>
<li>Apply a <strong>regression-based fallback</strong> to estimate speed.</li>
<li>Fallback equation: <code>launch_speed = -0.339 + 1.451 * speed / cos(PredictedAngle)</code> based on linear regression analysis of the average flight speed and ground truth initial velocity derived from video coding.</li>
<li><img src="https://i.imgur.com/S0mSCoX.png" alt="Predicted Launch Angles"></li>
</ul>
</li>
</ul>
</li>
<li><strong>Outputs</strong>:
<ul>
<li><code>RF_LaunchSpeed</code>: Speed computed using the random forest predicted angle.</li>
</ul>
</li>
</ul>
<h1 id="results">Results</h1>
<h3 id="predicted-launch-angles">Predicted Launch Angles</h3>
<p><img src="https://i.imgur.com/h1tq90r.png" alt="Predicted Launch Angles"><br>
<img src="https://i.imgur.com/IJhsUKW.png" alt="Predicted Launch Angles"><br>
These graphs show the predicted launch angle against the ground truth measurments for both the Random Forest and XGBoost models.</p>
<h3 id="predicted-launch-velocities-">Predicted Launch Velocities <img src="https://i.imgur.com/DW4KekZ.png" alt="Predicted Launch Velocities"></h3>
<p><img src="https://i.imgur.com/MyaF5E0.png" alt="Predicted Launch Velocities"><br>
This graph shows the distribution of predicted initial kick velocities against the ground turth.</p>
<h3 id="interactive-scatter-plot">Interactive Scatter Plot</h3>
<p>This interactive scatter plot provides a visualisation  of estimated launch speed and angle, and how these factors influence the errors when compared to ground truth measurments.</p>
<p>You can explore the plot interactively by visiting the link below:</p>
<p><a href="https://mc4713.github.io/plotly-hosting/interactive_scatter_plot.html">Interactive Scatter Plot</a></p>
<p>The models were trained on soccer kick data obtained at the StoneX stadium, performed by product testing staff. 275 separate events were recorded, however one kick had a negative ground truth initial velocity and so was excluded.</p>

