# MM_Edits
Motorsport Manager Edits, all edits work in FF22

## Random Setup for AI
In "SessionSetup.CreateAISetupForQualifyingAndRace()"
```C#
public void CreateAISetupForQualifyingAndRace()
{
	Mechanic mechanicOfDriver = this.mVehicle.driver.contract.GetTeam().GetMechanicOfDriver(this.mVehicle.driver);
	float randomWert = RandomUtility.GetRandom(0f, 1f);
	float num;
	if (mechanicOfDriver == null)
	{
		Debug.LogWarning("Could not find mechanic for driver, defaulting values", null);
		num = 1f;
	}
	else
	{
		num = mechanicOfDriver.GetStatsValue();
	}
	this.SetAISetupWithinRange(randomWert + 0.01f * num, 1f);
}
```
