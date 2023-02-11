---
layout: page
title: "Code Snippets"
course: "visstudiocameraunreal"
unit: 1
---

## Go To:

### [ ASplitScreenManager.cpp ](#asplitscreenmanagercpp)
### [ ASplitScreenManager.h ](#asplitscreenmanagerh)
### [ VizStudioGameInstance.cpp ](#vizstudiogameinstancecpp)
### [ VizStudioGameInstance.h ](#vizstudiogameinstanceh)
### [ Blur Material Custom Node ](#blur-material-custom-node)


### ASplitScreenManager.cpp

<pre><code style="background: white;">
// Fill out your copyright notice in the Description page of Project Settings.

#include "ASplitScreenManager.h"
#include "Engine/World.h"
#include "Engine/GameViewportClient.h"
#include "Kismet/GameplayStatics.h"
#include "Engine/LocalPlayer.h"

// Sets default values
AASplitScreenManager::AASplitScreenManager()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
}

// Called when the game starts or when spawned
void AASplitScreenManager::BeginPlay()
{
	Super::BeginPlay();
	ApplyEightViewportSettings();	
}

// Called every frame
void AASplitScreenManager::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);
}

void AASplitScreenManager::ApplyEightViewportSettings()
{
	UGameViewportClient* Viewport = GetWorld()->GetGameViewport();
	Viewport->MaxSplitscreenPlayers = 9;

	FSplitscreenData ScreenLayout;

	float W = 1.0f / 8.0f;
	float H = 1.0f;

	auto Screen1 = FPerPlayerSplitscreenData(W, H, -1 * W, 0 * H);
	auto Screen2 = FPerPlayerSplitscreenData(W, H, 0 * W, 0 * H);
	auto Screen3 = FPerPlayerSplitscreenData(W, H, 1 * W, 0 * H);
	auto Screen4 = FPerPlayerSplitscreenData(W, H, 2 * W, 0 * H);
	auto Screen5 = FPerPlayerSplitscreenData(W, H, 3 * W, 0 * H);
	auto Screen6 = FPerPlayerSplitscreenData(W, H, 4 * W, 0 * H);
	auto Screen7 = FPerPlayerSplitscreenData(W, H, 5 * W, 0 * H);
	auto Screen8 = FPerPlayerSplitscreenData(W, H, 6 * W, 0 * H);
	auto Screen9 = FPerPlayerSplitscreenData(W, H, 7 * W, 0 * H);

	ScreenLayout.PlayerData.Add(Screen1);
	ScreenLayout.PlayerData.Add(Screen2);
	ScreenLayout.PlayerData.Add(Screen3);
	ScreenLayout.PlayerData.Add(Screen4);
	ScreenLayout.PlayerData.Add(Screen5);
	ScreenLayout.PlayerData.Add(Screen6);
	ScreenLayout.PlayerData.Add(Screen7);
	ScreenLayout.PlayerData.Add(Screen8);
	ScreenLayout.PlayerData.Add(Screen9);

	Viewport->SetDisableSplitscreenOverride(true);
	Viewport->SplitscreenInfo[ESplitScreenType::None] = ScreenLayout;
}
</code></pre>


---

### ASplitScreenManager.h
```
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "ASplitScreenManager.generated.h"

UCLASS()
class <<<<PROJECT TITLE>>>>_API AASplitScreenManager : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	AASplitScreenManager();
	void ApplyEightViewportSettings();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

};
```

---

### VizStudioGameInstance.cpp
```
// Fill out your copyright notice in the Description page of Project Settings.

#include "VizStudioGameInstance.h"
#include "Engine/Engine.h"
#include "Engine/World.h"
#include "Engine/LocalPlayer.h"

ULocalPlayer* UVizStudioGameInstance::RequestLocalPlayer(int32 ControllerId, FString& OutError, bool bSpawnActor)
{
	check(GetEngine()->LocalPlayerClass != NULL);

	ULocalPlayer* NewPlayer = NULL;
	int32 InsertIndex = INDEX_NONE;

	const int32 MaxSplitscreenPlayers = 9; // This is what does the trick!

	if (FindLocalPlayerFromControllerId(ControllerId) != NULL)
	{
		OutError = FString::Printf(TEXT("A local player already exists for controller ID %d,"), ControllerId);
	}
	else if (LocalPlayers.Num() < MaxSplitscreenPlayers)
	{
		// If the controller ID is not specified then find the first available
		if (ControllerId < 0)
		{
			for (ControllerId = 0; ControllerId < MaxSplitscreenPlayers; ++ControllerId)
			{
				if (FindLocalPlayerFromControllerId(ControllerId) == NULL)
				{
					break;
				}
			}
			check(ControllerId < MaxSplitscreenPlayers);
		}
		else if (ControllerId >= MaxSplitscreenPlayers)
		{
			UE_LOG(LogPlayerManagement, Warning, TEXT("Controller ID (%d) is unlikely to map to any physical device, so this player will not receive input"), ControllerId);
		}

		NewPlayer = NewObject<ULocalPlayer>(GetEngine(), GetEngine()->LocalPlayerClass);
		InsertIndex = AddLocalPlayer(NewPlayer, ControllerId);
		if (bSpawnActor && InsertIndex != INDEX_NONE && GetWorld() != NULL)
		{
			if (GetWorld()->GetNetMode() != NM_Client)
			{
				// server; spawn a new PlayerController immediately
				if (!NewPlayer->SpawnPlayActor("", OutError, GetWorld()))
				{
					RemoveLocalPlayer(NewPlayer);
					NewPlayer = NULL;
				}
			}
			else
			{
				// client; ask the server to let the new player join
				//NewPlayer->SendSplitJoin();
			}
		}
	}
	else
	{
		OutError = FString::Printf(TEXT("Maximum number of players (%d) already created.  Unable to create more."), MaxSplitscreenPlayers);
	}

	if (OutError != TEXT(""))
	{
		UE_LOG(LogPlayerManagement, Log, TEXT("UPlayer* creation failed with error: %s"), *OutError);
	}

	return NewPlayer;
}
```

---

### VizStudioGameInstance.h
```
#pragma once

#include "CoreMinimal.h"
#include "Engine/GameInstance.h"
#include "VizStudioGameInstance.generated.h"

UCLASS()
class VIZTESTING_API UVizStudioGameInstance : public UGameInstance
{
	GENERATED_BODY()

public:

	UFUNCTION(BlueprintCallable)
		ULocalPlayer * RequestLocalPlayer(int32 ControllerId, FString& OutError, bool bSpawnActor);
};
```

---

### Blur Material Custom Node
```
float3 res = 0;

//new - get invSize from here
float2 invSize = View.ViewSizeAndInvSize.zw;

//new - we need to fix uv coordinates like this (still seems to be a bug in 4.21)
uv = ViewportUVToSceneTextureUV(uv,14);

int TexIndex = 14;
float weights[] = { 0.01, 0.02, 0.04, 0.02, 0.01, 0.02, 0.04, 0.08, 0.04, 0.02, 0.04, 0.08, 0.16, 0.08, 0.04, 0.02, 0.04, 0.08, 0.04, 0.02, 0.01, 0.02, 0.04, 0.02, 0.01};

float offsets[] = { -4, -2, 0, 4, 2 };

if(Mask <= 0.01) return SceneTextureLookup(uv, TexIndex, false);

uv *= 0.5;
for (int i = 0; i < 5; ++i)
{
float v = uv.y + offsets[i] * invSize.y * Mask * BlurStrength;
int temp = i * 5;
for (int j = 0; j < 5; ++j)
{
float u = uv.x + offsets[j] * invSize.x * Mask * BlurStrength;
float2 uvShifted = uv + float2(u, v);
float weight = weights[temp + j];
float3 tex = SceneTextureLookup(uvShifted, TexIndex, false);
res += tex * weight;
}
}

return float4(res, 1);
```