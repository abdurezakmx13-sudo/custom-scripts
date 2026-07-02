//VERSION=3

function setup() {
  return {
    input: ["B02","B04","B08","B11","B12"],
    output: { bands: 3 }
  };
}

function evaluatePixel(s) {

  // Indices
  let iron = s.B04 / s.B02;
  let clay = s.B11 / s.B12;
  let gossan = s.B11 / s.B04;
  let ndvi = (s.B08 - s.B04) / (s.B08 + s.B04);

  // Mask vegetation
  if (ndvi > 0.25) {
    return [0,0,0];
  }

  // Gold prospectivity score
  let score = 0;

  if (iron > 1.6) score++;
  if (clay > 1.1) score++;
  if (gossan > 1.8) score++;

  // Colors
  if (score == 3)
    return [1,0,0];        // Red = Very High

  if (score == 2)
    return [1,1,0];        // Yellow = High

  if (score == 1)
    return [0,1,0];        // Green = Moderate

  return [0.2,0.2,0.2];    // Gray = Low
}
