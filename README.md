# TB-Universal-code
# Last update - 26.7.2018
Isn'T necesary contain every code, just what you want use

But must contain addition data   --  customized for the map

#######  Example  ########
----------------    MAIN.scr

starting
begin
  ifThrowback = true;
  newThrowbackVariables;

  GT_DoubleNat = [4,14,24];
  GT_DoublePeople = [3,13,23];
  GT_TripleNat = [5],15,26;
  GT_Survival = [24,23,22,21,26];
  GT_Mine = [6];

  enable_human_prediction:=true;
  Disable_All_Games;

  mp_selectmsg = true;

  read_multiplayer_and_game_parameters;
  specialParameters;
  standartSets;
  prepare_map_coordinates;

  InitGameRules;

  set_shared_visions_and_alliances;
  ResetFog;
  ClearAllFogForSide(Player_Side);

  init_map;

  if Game_Type in GT_DoubleNat^GT_TripleNat then
    modify_nations;

  if not def_game_rules then
  begin
    Start_tech_ur;
    enable(30);
  end;

  prepare_sides;
  InitLimits;

  if  def_kings_age then
    init_kings_counting;

  init_respawn;
  Init_Win_Condition;

  mastodonti;
  Start_Zvirata;

  init_Daily;

  your_side:=Player_Side;
  music_nat:=Side_Nations[your_side];

  if IAmSpec then
    begin
      FogOff(true);
      CenterNowOnXY(104, 69);
      Enable(400);
    end
  else
    begin
      //for i=1 to 8 do
      //ClearAllFogForSide(your_side);
      CenterNowOnXY(start_def[Side_Positions[your_side]][1],start_def[Side_Positions[your_side]][2]);
      Init_Win_Condition;
    end;

  if def_builduptime then
    BeginBuildUp;

  Anticheatcheck;
//  ShowResources(Cela_mapa,def_siberite_detection,15);

  wait(35);
  InitMultiplayerTime;

end;

function Disable_All_Games;
begin
  Disable_Standard_Games;
end;

function specialParameters;
begin
  if not ifThrowback then
  begin
    def_game_rules = 0;
    def_public_score = 1;
    if not game_type in GT_Survival then
    begin
      def_siberite_detection = 1;
      def_amount_of_tiger = def_amount_of_apemen;
      def_daily_start = rand(0,4);
    end;
  end;
end;

function prepare_map_coordinates;
begin
  depot_def:=[ [56,66,1], [49,36,1], [107,23,3], [138,29,3], [99,130,0], [132,136,0], [194,122,5], [184,95,4],   ] ;
  oil_deposits_locations:=     [ [43,57], [41,43], [110,13], [125,19], [113,140], [128,143], [197,114], [186,102], ];
  siberite_deposits_locations:=[ [52,58],  [49,48],  [111,20], [125,27], [113,132], [125,135], [189,110], [193,101], ];
  extra_oil_deposits_locations:=[ [42,77], [83,6], [104,153], [221,132] ];
  extra_sib_deposits_locations:=[ [16,25], [136,5], [154,151], [194,79], ];

  for_show_near_resources = [ [ [[43,57]]             ,[[52,58]] ],
                              [ [[41,43]]             ,[[49,48]] ],
                              [ [[110,13]]            ,[[111,20]] ],
                              [ [[125,19]]            ,[[125,27]] ],
                              [ [[113,140]]           ,[[113,132]] ],
                              [ [[128,143]]           ,[[125,135]] ],
                              [ [[197,114]]           ,[[189,110]] ],
                              [ [[186,102]]           ,[[193,101]] ],  ];
  case def_base_level of
   0:begin // nothing
       buildings_def=[[],[],[],[],[],[],[],[]];
     end;
   1:begin // depot only
       buildings_def=[[],[],[],[],[],[],[],[]];
     end;
   2:begin
       buildings_def=[[],[],[],[],[],[],[],[]];
     end;
   3:begin // small base
      buildings_def=[[],[],[],[],[],[],[],[]];
     end;
  4:begin // big base             
       buildings_def=[
                     [[b_oil_mine,43,57,4],[b_lab,49,65,1],[b_factory,58,57,2],[b_barracks,71,71,5],[b_bunker,64,77,5],[b_bunker,67,57,4],[b_ext_gun,62,57,4],[b_ext_track,54,53,2],[b_oil_power,46,57,0],[b_siberite_power,49,58,3],[b_siberite_mine,52,58,3]],
                     [[b_oil_mine,41,43,4],[b_lab,37,36,1],[b_factory,54,45,1],[b_barracks,56,32,3],[b_bunker,41,27,3],[b_bunker,64,49,5],[b_ext_gun,58,49,5],[b_ext_track,50,40,1],[b_oil_power,44,45,4],[b_siberite_power,38,44,4],[b_siberite_mine,49,48,0]],

                     [[b_oil_mine,110,13,0],[b_lab,98,19,2],[b_factory,115,31,4],[b_barracks,106,38,0],[b_bunker,96,27,1],[b_bunker,118,40,5],[b_ext_gun,115,35,0],[b_ext_track,118,34,5],[b_oil_power,110,17,3],[b_siberite_power,113,13,3],[b_siberite_mine,111,22,1]],
                     [[b_oil_mine,125,19,3],[b_lab,129,25,2],[b_factory,128,35,1],[b_barracks,145,40,5],[b_bunker,144,27,4],[b_bunker,129,43,0],[b_ext_gun,128,38,0],[b_ext_track,128,31,3],[b_oil_power,124,21,3],[b_siberite_power,123,16,3],[b_siberite_mine,125,27,4]] ,

                     [[b_oil_mine,113,140,4],[b_lab,110,137,5],[b_factory,108,125,4],[b_barracks,91,115,2],[b_bunker,89,127,1],[b_bunker,103,113,3],[b_ext_gun,112,125,4],[b_ext_track,104,121,2],[b_oil_power,114,135,4],[b_siberite_power,111,142,3],[b_siberite_mine,113,132,5]],
                     [[b_oil_mine,128,143,4],[b_lab,140,138,5],[b_factory,120,125,1],[b_barracks,128,119,3],[b_bunker,118,117,2],[b_bunker,141,129,4],[b_ext_gun,120,121,3],[b_ext_track,116,125,1],[b_oil_power,127,138,1],[b_siberite_power,141,143,1],[b_siberite_mine,125,135,2]],

                     [[b_oil_mine,197,114,0],[b_lab,203,122,4],[b_factory,182,108,3],[b_barracks,188,127,0],[b_bunker,201,132,5],[b_bunker,178,115,0],[b_ext_gun,182,104,3],[b_ext_track,179,105,2],[b_oil_power,194,113,3],[b_siberite_power,196,116,3],[b_siberite_mine,189,110,1]],
                     [[b_oil_mine,186,102,4],[b_lab,188,90,3],[b_factory,174,98,0],[b_barracks,165,89,2],[b_bunker,171,81,2],[b_bunker,170,103,1],[b_ext_gun,174,102,0],[b_ext_track,177,101,5],[b_oil_power,188,101,4],[b_siberite_power,190,100,3],[b_siberite_mine,193,101,4]]
                     ];
     end;
  end;

  Cela_mapa = celamapa;
  king_territory = kral;
  oblasti_beden := [Bedny1,Bedny2,Bedny3,Bedny4,Bedny5,Bedny6,Bedny7,Bedny8];
  dalsi_oblasti_beden := [dalsi_bedny];
  koof_bedny = 2.5;

  BuildUpAreas:= [base1,base2,base3,base4,base5,base6,Base7,Base8];
end;

export function SimMPDefinition;
begin
      Game_Type=1;

      Player_Side    = 3;
      Player_Team    = 0;

      Side_Positions = [1,2,3,4,5,6,7,7];
      Side_Teams     = [1,1,2,2,3,3,4,4];
      Side_Nations   = [1,2,2,1,3,2,1,2];
      Side_Comps     = [0,0,0,0,0,0,0,0];
      Teams          = [[1,2],[3,4],[5,6],[7,8]];
end;

export function SimMPOptions;
begin
      def_game_rules                   =0;
      def_base_level                   =0;
      def_amount_of_people             =5;                     
      def_amount_of_apemen_in_team     =2;
      def_initial_level                =1;
      def_classes                      =0;
      def_starting_resources           =2;
      def_shipments_density            =2;
      def_extra_oil_deposits           =1;
      def_extra_sib_deposits           =1;
      def_shared_vision                =1;
      def_morale_flags                 =1;
      def_win_rules                    =2;
      def_amount_of_apemen             =1;
      def_amount_of_tiger              =1;
      def_respawining_type             =1;
      def_people_respawning            =1;
      def_tech_level                   =8;
      def_upgrade_tl                   =1;
      def_siberite_detection           =1;
      def_siberite_bomb                =3;
      def_builduptime                  =1;
      def_desert_warior                =2;
      def_daily_start                  =1;
      def_daily_speed                  =3;
      def_public_score                 =0;
      def_kings_age                    =2;
      def_warForTerritory              =4;

end;


export function Hustota_Zasilek;
var i;
begin
  pocet_hracu := 0;

  for i:=1 to 8 do
    if Side_Positions[i] <> 0 then
       pocet_hracu := pocet_hracu + 1;
  if game_type = GT_doublenat^GT_doublepeople then
    pocet_hracu = pocet_hracu * 2;

  if def_game_rules then
     tech_level=8;

  if pocet_hracu = 2 then
    case def_shipments_density of
      0: shipments_density := Tech_Level*1.5 + 4;
      1: shipments_density := Tech_Level*2 + 6;
      2: shipments_density := Tech_Level*3 + 8;
    end
  else if pocet_hracu <= 4 then
    case def_shipments_density of
      0: shipments_density := Tech_Level*2.75 + 6;
      1: shipments_density := Tech_Level*4.25 + 8;
      2: shipments_density := Tech_Level*6 + 9;
    end
  else if pocet_hracu <= 6 then
    case def_shipments_density of
      0: shipments_density := Tech_Level*3.75 + 8;
      1: shipments_density := Tech_Level*6 + 9;
      2: shipments_density := Tech_Level*8.25 + 10;
    end
  else if pocet_hracu <= 8 then
    case def_shipments_density of
      0: shipments_density := Tech_Level*4.5 + 9;
      1: shipments_density := Tech_Level*7.25 + 10;
      2: shipments_density := Tech_Level*11.25 + 12;
    end;
end;



----------------   Events.scr
//volání techů pro nové techy
on ResearchComplete(t,s) do
begin
  if not def_Game_Rules and ifThrowback then
   AllowNewTech(t,getside(s));

end;

// Rušení příkazů pro Game Rules
on VehicleConstructionStarted(factory, chassis, engine, control, weapon) do
  RestrictVehicleConstructionStarted(factory, chassis, engine, control, weapon);
on WeaponPlaced(building, weapon) do
  RestrictWeaponPlaced(building, weapon);

// Jména zákldaden a věci pro Game Rules
on BuildingStarted(b, h) do
begin
  if def_game_rules then
  begin
    ExecuteLimits(b, GetBType(b), GetSide(b), 0, 1);
  end;               

  if GetBType (b) in [b_depot,b_warehouse] then
    NameBuildingStarted(b, h);
end;


On BuildingComplete(b) do
begin
  if GetBType (b) in [b_depot,b_warehouse] then
    NameBuildingComplete(b);
end;

On BuildingCaptured(b,o,e) do
begin
  if def_game_rules then
  begin
    ExecuteLimits(b, GetBType(b), GetSide(b), 0, 1);
  end;

  if GetBType (b) in [b_depot,b_warehouse] then
    NameBuildingCaptured(b,o,e);
end;

on UnitDestroyed(j) do
var x,y,pos,dir;
begin
 
//  if GetBType(j) in [b_depot, b_warehouse] then
//    CallKillBattleFlag(j);

  if def_game_rules then
    CallExecuteLimits(j);

end;

on VehicleCaptured(j, i1, o, i2) do
begin
  if def_game_rules and (GetChassis(j) = 25) then
    ExecuteLimits(j, b_behemoth, GetSide(j), o, 1);
end;

on HumanDestroyed(identifier, side, nation, x, y, direction, sex, cl) do
 if def_respawining_type > 1 then
  RespawnHumanDestroyed(identifier, side, nation, x, y, direction, sex, cl);
