Build at runtime
================

Build a random dungeon at runtime.  This assumes that you have a dungeon in your level with the themes already setup.

The code sample is placed in the GameMode class on StartPlay


**GameMode Header file**

```
// Copyright 1998-2017 Epic Games, Inc. All Rights Reserved.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "DA416XGameMode.generated.h"

UCLASS(minimalapi)
class ADA416XGameMode : public AGameModeBase
{
	GENERATED_BODY()

public:
	ADA416XGameMode();

	virtual void StartPlay() override;
};
```



**GameMode Source file**

<pre>
// Copyright 1998-2017 Epic Games, Inc. All Rights Reserved.

#include "DA416XGameMode.h"
#include "DA416XCharacter.h"
#include "UObject/ConstructorHelpers.h"

#include "EngineUtils.h"		// for TActorIterator<b>
#include "Dungeon.h"
#include "GridDungeonConfig.h"
</b>

ADA416XGameMode::ADA416XGameMode()
{
	// ...
}

void ADA416XGameMode::StartPlay()
{
	// Find the dungeon actor from the scene
	TActorIterator<ADungeon> DungeonIter = TActorIterator<ADungeon>(GetWorld());

	if (DungeonIter) {
		ADungeon* Dungeon = *DungeonIter;

		// grab the config so we can change it before building
		UDungeonConfig* Config = Dungeon->GetConfig();

		// If we want to set a specific builder config (like grid / floorplan etc.) cast to the appropriate config object
		if (UGridDungeonConfig* GridConfig = Cast<UGridDungeonConfig>(Config)) {
			GridConfig->Seed = FMath::Rand();	// Generate a new dungeon every time with a random seed
			GridConfig->NumCells = 100;			// Set a grid config specific property
			// Set you other properties here
		}

		// Build the dungeon
		Dungeon->BuildDungeon();
	}
}


</pre>


