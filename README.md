- 👋 Hi, I’m @wake2200
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
wake2200/wake2200 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

Servo servoverti;
int servov = 0;
int servovLimitHigh = 160;
int servovLimitLow = 20;
//Assigning LDRs
int ldrtopl = A0; //top left LDR green
int ldrtopr = A3; //top right LDR yellow
int ldrbotl = A1; // bottom left LDR blue
int ldrbotr = A2; // bottom right LDR orange

void setup ()
{
  servohori.attach(10);
  servohori.write(45);
  servoverti.attach(9);
  servoverti.write(45);
  Serial.begin(9600);
  delay(500);
}

void loop()
{
  servoh = servohori.read();
  servov = servoverti.read();
  //capturing analog values of each LDR
  int topl = analogRead(ldrtopl);
  int topr = analogRead(ldrtopr);
  int botl = analogRead(ldrbotl);
  int botr = analogRead(ldrbotr);
  // calculating average
  int avgtop = (topl + topr) / 2; //average of top LDRs
  int avgbot = (botl + botr) / 2; //average of bottom LDRs
  int avgleft = (topl + botl) / 2; //average of left LDRs
  int avgright = (topr + botr) / 2; //average of right LDRs
  Serial.println(avgtop);

  if (TOLERANCE < avgbot - avgtop)
  {
    servoverti.write(servov + 1);
    if (servov > servovLimitHigh)
    {
      servov = servovLimitHigh;
    }
    delay(STEP_DELAY);
  }

  else if (TOLERANCE < avgtop - avgbot)
  {
    servoverti.write(servov - 1);
    if (servov < servovLimitLow)
    {
      servov = servovLimitLow;
    }
    delay(STEP_DELAY);
  }

  else
  {
    servoverti.write(servov);
  }

  if (avgleft - avgright > TOLERANCE)
  {
    servohori.write(servoh + 1);

    if (servoh > servohLimitHigh)
    {
      servoh = servohLimitHigh;
    }
    delay(STEP_DELAY);
  }

  else if (avgright - avgleft > TOLERANCE)
  {
    servohori.write(servoh - 1);

    if (servoh < servohLimitLow)
    {
      servoh = servohLimitLow;
    }
    delay(STEP_DELAY);
  }

  else
  {
    servohori.write(servoh);
  }
  delay(STEP_DELAY);
}
