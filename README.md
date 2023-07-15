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

## AI will not promote
In "Championship.ProcessChampionshipPromotions(bool)"

"bool flag" needs to be set to "false"
```C#
public void ProcessChampionshipPromotions(bool inPlayerAcceptPromotion = true)
{
	Championship nextTierChampionship = this.GetNextTierChampionship();
	bool flag = false;
	int num = 0;
	if (nextTierChampionship != null && nextTierChampionship.championshipPromotions.lastPlace.IsPlayersTeam())
	{
		Game.instance.dialogSystem.OnPlayerTeamRelegatable(flag, this.championshipID);
	}
	if (nextTierChampionship != null && flag)
	{
		num++;
		Team lastPlace = nextTierChampionship.championshipPromotions.lastPlace;
		nextTierChampionship.championshipPromotions.lastPlaceStatus = ChampionshipPromotions.Status.Relegated;
		nextTierChampionship.RemoveTeamEntry(lastPlace);
		this.RemoveTeamEntry(this.mChampionshipPromotions.champion);
		this.mChampionshipPromotions.championStatus = ChampionshipPromotions.Status.Promoted;
		nextTierChampionship.SetPromotedTeam(this.mChampionshipPromotions.champion, this, this.mChampionshipPromotions.championPartRankings);
		nextTierChampionship.AddTeamEntry(this.mChampionshipPromotions.champion);
		this.SetRelegatedTeam(lastPlace, nextTierChampionship, nextTierChampionship.championshipPromotions.lastPlacePartRankings);
		this.AddTeamEntry(lastPlace);
		nextTierChampionship.standings.UpdateStandings();
		this.standings.UpdateStandings();
	}
	else
	{
		this.mChampionshipPromotions.championStatus = ChampionshipPromotions.Status.RefusedPromotion;
		if (nextTierChampionship != null)
		{
			nextTierChampionship.mChampionshipPromotions.championStatus = ChampionshipPromotions.Status.SavedFromRelegation;
		}
	}
	this.completedPromotions = true;
}
```

## No engine or gearbox wear
In "Racingvehicle.OnSessionEnd
```C#
public void OnSessionEnd()
	{
		if (Game.instance.sessionManager.sessionType == SessionDetails.SessionType.Race)
		{
			for (int i = 0; i < this.mDriversForCar.Length; i++)
			{
				this.mDriversForCar[i].driverForm.RecordAverageForm(base.championship.GetCurrentEventDetails());
				this.mDriversForCar[i].driverStamina.Reset();
			}
		}
	}
```
