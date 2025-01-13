---


---

<h1 id="soccer--launch-velocity-algorithm">Soccer  Launch Velocity Algorithm</h1>
<h2 id="overview">Overview</h2>
<p>This algorithm is designed to estimate the <strong>launch angle</strong> and <strong>initial speed</strong> of a soccer ball based on UWB (Ultra-Wideband) tracking data and IMU derived ball flight features.<br>
The process involves:</p>
<ol>
<li><strong>Angle Approximation</strong>: Predicting the ball’s launch angle using machine learning models based on the duration, speed and spin of the ball during flight.</li>
<li><strong>Speed Estimation</strong>: Computing the initial velocity using trajectory fitting or regression-based fallback methods if the flight duration is too short; less than 0.5 seconds.</li>
</ol>
<hr>
<h3 id="angle-approximation">Angle Approximation</h3>
<ul>
<li><strong>Objective</strong>: Estimate the launch angle of the ball for each event.</li>
<li><strong>Inputs</strong>:
<ul>
<li>Event features for each flight: duration, <code>speed</code>, and <code>spin</code> are extracted from the ball IMU data.</li>
<li>Two pre-trained machine learning models have been prepared:
<ul>
<li><strong>Random Forest (RF)</strong></li>
<li><strong>XGBoost (XGB)</strong><br>
However XGBoost requires considerably more data for accurate estimation, therefore the random forest model is more robust against overfitting and has been used more exclusively so far.</li>
</ul>
</li>
</ul>
</li>
<li><strong>Procedure</strong>:
<ul>
<li>Pass event features through the RF and XGB models to predict the launch angle.</li>
<li>Store predictions in the dataset:
<ul>
<li><code>RF_PredictedAngle</code>: Predicted angle from the Random Forest model.</li>
<li><code>XGB_PredictedAngle</code>: Predicted angle from the XGBoost model.</li>
</ul>
</li>
</ul>
</li>
<li><strong>Outputs</strong>: Predicted angles for each event are added to the dataframe, ready for use in subsequent speed estimation.</li>
</ul>
<hr>
<h3 id="speed-estimation">Speed Estimation</h3>
<ul>
<li><strong>Objective</strong>: Calculate the ball’s initial velocity during flight.</li>
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
<li>If polynomial fitting fails (e.g., insufficient data):
<ul>
<li>Apply a <strong>regression-based fallback</strong> to estimate speed.</li>
<li>Fallback equation: <code>launch_speed = -0.339 + 1.451 * speed / cos(PredictedAngle)</code>.</li>
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
<h2 id="summary">Summary</h2>

<table>
<thead>
<tr>
<th></th>
<th>Input</th>
<th>Output</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Angle Approximation</strong></td>
<td>Event features (<code>hangTime</code>, <code>speed</code>, <code>spin</code>)</td>
<td><code>RF_PredictedAngle</code>, <code>XGB_PredictedAngle</code></td>
</tr>
<tr>
<td><strong>Speed Estimation</strong></td>
<td>UWB arrays (<code>tUWB</code>, <code>posX</code>, <code>posY</code>), predicted angles</td>
<td><code>RF_LaunchSpeed</code></td>
</tr>
</tbody>
</table><p><a href="https://imgur.com/vbkRise">Predicted Launch Angles</a></p>
<p><a href="https://imgur.com/BJbeAoS">Predicted Launch Velocities </a></p>

