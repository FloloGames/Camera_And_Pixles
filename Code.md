import processing.video.*;
//Easy ;-)
Capture cam;

PImage prev;

float gen = 25;

color stand = color(random(255), random(255), random(255));
color walk = color(random(255), random(255), random(255));

void setup() {
  size(640, 480);
  cam = new Capture(this, 640, 480);
  cam.start();
  prev = createImage(640, 480, RGB);
}
void draw() {
  image(cam, 0, 0);
  loadPixels();
  cam.loadPixels();
  for (int x = 0; x < width; x++) {
    for (int y = 0; y < height; y++) {
      int loc = x+y*width;
      color currentColor = color(cam.pixels[loc]);
      float r1 = red(currentColor);
      float g1 = green(currentColor);
      float b1 = blue(currentColor);
      color prevColor = color(prev.pixels[loc]);
      float r2 = red(prevColor);
      float g2 = green(prevColor);
      float b2 = blue(prevColor);
      float dist = dist(r1, g1, b1, r2, g2, b2);
      if (dist < gen) {
        pixels[loc] = stand;
      } else {
        pixels[loc] = walk;
      }
    }
  }
  updatePixels();
}
void keyPressed() {
  if (keyCode == ENTER) {
    saveFrame("Image-####.jpg");
  } else if (key == ' ') {//SPACE
    stand = color(random(255), random(255), random(255));
    walk = color(random(255), random(255), random(255));
  }
}
void captureEvent(Capture cam) {
  prev.copy(cam, 0, 0, cam.width, cam.height, 0, 0, prev.width, prev.height);
  prev.updatePixels();
  cam.read();
}
