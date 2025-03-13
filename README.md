<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Color Prediction</title>
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
            margin-top: 50px;
        }
        #prediction {
            font-size: 24px;
            font-weight: bold;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>AI Color Prediction</h1>
    <button onclick="predictColor()">Predict Color</button>
    <div id="prediction">Click the button to predict.</div>

    <script>
        // Define possible colors
        const colors = ["Red", "Green", "Blue"];
        
        // Create a simple AI model
        const model = tf.sequential();
        model.add(tf.layers.dense({units: 3, inputShape: [3], activation: 'softmax'}));
        model.compile({optimizer: 'adam', loss: 'categoricalCrossentropy'});

        // Generate sample training data
        const xs = tf.tensor2d([
            [1, 0, 0], // Red
            [0, 1, 0], // Green
            [0, 0, 1], // Blue
            [1, 1, 0], // Red-Green
            [0, 1, 1], // Green-Blue
            [1, 0, 1]  // Red-Blue
        ]);
        const ys = tf.tensor2d([
            [1, 0, 0], // Red
            [0, 1, 0], // Green
            [0, 0, 1], // Blue
            [1, 0, 0], // Red
            [0, 1, 0], // Green
            [0, 0, 1]  // Blue
        ]);

        // Train model
        async function trainModel() {
            await model.fit(xs, ys, {epochs: 100});
            console.log("Training complete.");
        }
        trainModel();

        // Predict a color
        async function predictColor() {
            const input = tf.tensor2d([[
                Math.random(), Math.random(), Math.random()
            ]]); 
            const prediction = model.predict(input);
            const index = prediction.argMax(1).dataSync()[0];
            document.getElementById("prediction").innerText = "Predicted Color: " + colors[index];
        }
    </script>
</body>
</html>
