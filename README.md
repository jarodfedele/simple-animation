# Oscillating Circle Test

This repository is a minimal reproduction of an intermittent animation problem. A solid circle
moves continuously between the left and right edges of the viewport, but it can sometimes appear
to hold briefly and then catch up despite using a compositor-friendly CSS transform. The stuttering appears to occur unpredictably and randomly.