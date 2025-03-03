# -Adaptive-Traffic-Light-Control-System
int measureDistance() {
 digitalWrite(trigPin, LOW);
 delayMicroseconds(2);
 digitalWrite(trigPin, HIGH);
 delayMicroseconds(10);
 digitalWrite(trigPin, LOW);
 long duration = pulseIn(echoPin, HIGH);
 int distance = duration / 58.2;
 Serial.print("Distance: ");
 Serial.print(distance);
 Serial.println(" cm");
 return distance;
 }
 void displayDigit(int digit) {
 int segmentState = segmentMap[digit];
 for (int i = 0; i < 7; i++) {
 digitalWrite(segments[i], bitRead(segmentState, i) ? LOW : HIGH); 
// Common Anode 7-segment display
 }
 }
 void startTrafficSequence() {
 // Green LED sequence from 9 to 5 seconds
 for (int i = 9; i >= 0; i--) {
 displayDigit(i); 
/
 / Show the current digit on the 7-segment display
 if (i >= 5) {
 digitalWrite(greenLED, HIGH); 
// Green light glows from 9 to 5
 digitalWrite(yellowLED, LOW);
 } else {
 digitalWrite(greenLED, LOW);
 digitalWrite(yellowLED, HIGH); /
 / Yellow light glows from 4 to 0
 }
 delay(1000); 
// Wait 1 second for each count
 // Check if an object appears within 5 cm during the
 countdown
 if (measureDistance() <= 5) {
 // Stop countdown if object detected
 digitalWrite(greenLED, LOW);
 digitalWrite(yellowLED, LOW);
 digitalWrite(redLED, HIGH); 
// Red LED on only
 clearDisplay(); 
// Clear display while object is detected
 return; 
// Exit the function to maintain red light
 }
 }
