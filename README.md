using System;
using System.Collections.Generic;
using UnityEngine;
using SFS.Parts;
using SFS.Parts.Modules;
using SFS.Variables;
using ItemData = SFS.Parts.PartData;

namespace StarshipMod
{
    // Dit registreert jouw mod in de SFS ModLoader
    public class Main : ModLoader.Mod
    {
        public override string ModNameID => "spacex.starship.pack";
        public override string DisplayName => "SpaceX Starship Full Pack";
        public override string Author => "Timmy";
        public override string ModVersion => "1.0.0";
        public override string MinimumGameVersionRequired => "1.5.9.8";

        // Deze lijst houdt de custom Starship onderdelen bij
        public static List<PartData> StarshipParts = new List<PartData>();

        public override void Early_Load()
        {
            Debug.Log("[Starship Mod] Bezig met het genereren van Starship onderdelen...");
            
            // Initialiseer en maak de onderdelen aan via code
            CreateStarshipTank();
            CreateRaptorEngine();
            
            // Voeg de onderdelen toe aan de game database
            foreach (PartData part in StarshipParts)
            {
                Base.partsLoader.parts.Add(part.name, part);
                Debug.Log($"[Starship Mod] {part.name} succesvol toegevoegd aan de bouwlijst!");
            }
        }

        public override void Load()
        {
            // Eventuele logica voor wanneer de mod volledig actief is in de wereld
            Debug.Log("[Starship Mod] Starship Pack is klaar voor gebruik!");
        }

        // --- HIERONDER WORDEN DE ONDERDELEN IN DE CODE GEBOUWD ---

        private void CreateStarshipTank()
        {
            // Maak een nieuw PartData object aan voor de Starship RVS Tank
            PartData starshipTank = ScriptableObject.CreateInstance<PartData>();
            starshipTank.name = "Starship_Main_Tank";
            starshipTank.displayName = "Starship Stainless Steel Tank";
            starshipTank.description = "The main propellant tank for the Starship upper stage, made of shiny stainless steel.";
            starshipTank.mass = 5.0f; // Gewicht in tonnen

            // Voeg een brandstofcomponent (ResourceModule) toe aan de tank via code
            ResourceModule fuelComponent = starshipTank.gameObject.AddComponent<ResourceModule>();
            fuelComponent.resourceType = ResourceType.LiquidFuel; // Gebruikt vloeibare brandstof
            fuelComponent.capacity = 1200.0f; // Tankinhoud
            fuelComponent.amount = 1200.0f;   // Startinhoud

            // Voeg het onderdeel toe aan onze mod-lijst
            StarshipParts.Add(starshipTank);
        }

        private void CreateRaptorEngine()
        {
            // Maak een nieuw PartData object aan voor de Raptor Motor
            PartData raptorEngine = ScriptableObject.CreateInstance<PartData>();
            raptorEngine.name = "Raptor_Engine_3";
            raptorEngine.displayName = "Raptor 3 Rocket Engine";
            raptorEngine.description = "Highly efficient full-flow staged combustion engine burning Methalox.";
            raptorEngine.mass = 1.6f;

            // Voeg een motorcomponent (EngineModule) toe
            EngineModule engineComponent = raptorEngine.gameObject.AddComponent<EngineModule>();
            engineComponent.thrust = 280.0f; // Stuwkracht
            engineComponent.isp = 350.0f;    // Efficiëntie (Specifieke impuls)

            // Voeg het onderdeel toe aan onze mod-lijst
            StarshipParts.Add(raptorEngine);
        }
    }
}
