---
layout: page
title: "C++ Code Snippets"
course: "visstudiocameraunreal"
unit: 1
---

## ASplitScreenManager.cpp

```
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
```
