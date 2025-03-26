;============================================================================================================================
; 																															=
; 																															=
;		 										Sun Tzu v0.2a - 1st August 2023												=
; 														by Arandi															=
; 																															=
; 																															=
;============================================================================================================================

;User Patch Constants
(load "Sun Tzu 0.2a/UserPatchConst")

;region Defconst
(defconst scout-type -286) ; To be improved for Meso civs

(defconst builder-male 118)
(defconst forager-male 120)
(defconst hunter-male 122)
(defconst lumberjack-male 123)
(defconst builder-female 212)
(defconst hunter-female 216)
(defconst lumberjack-female 218)
(defconst forager-female 354)
(defconst shepherd-male 590)
(defconst gold-miner-male 579)
(defconst gold-miner-female 581)
(defconst shepherd-female 592)
(defconst forage-class 907)
(defconst prey-animal-class 909)
(defconst predator-animal-class 910)
(defconst tree-class 915)
(defconst scout-cavalry-class 947)
;endregion

;region Goals
(defconst twenty-five-seconds-timer 1)
(defconst three-second-timer 2)
(defconst one-minute-timer 3)
(defconst ten-seconds-timer 4)
(defconst t-misc 5)

(defconst gl-local-total 51)
(defconst gl-local-last 52)
(defconst gl-remote-total 53)
(defconst gl-remote-last 54)
(defconst gl-starting-tc-x 55)
(defconst gl-starting-tc-y 56)
(defconst gl-sheep-tc-gathering-x 57)
(defconst gl-sheep-tc-gathering-y 58)
(defconst gl-sheep-gathering-x 59)
(defconst gl-sheep-gathering-y 60)

(defconst gl-temp-one 70)
(defconst gl-temp-two 71)
(defconst gl-temp-three 72)
(defconst gl-temp-four 73)
(defconst gl-temp-five 74)
(defconst gl-temp-six 75)
(defconst gl-temp-seven 76)
(defconst gl-temp-eight 77)
(defconst gl-temp-nine 78)
(defconst gl-temp-ten 79)
(defconst gl-temp-eleven 80)
(defconst gl-temp-twelve 81)
(defconst gl-temp-thirteen 82)
(defconst gl-temp-fourteen 83)
(defconst gl-temp-fifteen 84)
(defconst gl-temp-sixteen 85)
(defconst gl-temp-seventeen 86)
(defconst gl-temp-eighteen 87)
(defconst gl-temp-nineteen 88)
(defconst gl-temp-twenty 89)
(defconst gl-scouting-direction 90)

(defconst gl-first-deer-id 91)
(defconst gl-second-deer-id 92)
(defconst gl-third-deer-id 93)
(defconst gl-first-boar-id 94)
(defconst gl-second-boar-id 95)
(defconst gl-game-stage 96)
(defconst gl-target-one-x 97)
(defconst gl-target-one-y 98)
(defconst gl-temp-x 99)
(defconst gl-temp-y 100)
(defconst gl-scout-id-one 101)
(defconst gl-enemy-number 102)
(defconst gl-enemy-tc-x 103)
(defconst gl-enemy-tc-y 104)
(defconst gl-scouting-goal 105)
(defconst gl-primary-military-focus 106) ;goal that affects army composition, military buildings and research; 1 - militia-line, 2 - pikemen, 3 - archers, 4 - skirmishers, 5 - scouts, 6 - knights
(defconst gl-secondary-military-focus 107) ;like above
(defconst gl-tertiary-military-focus 108) ;like above
(defconst gl-enemy-infantry 109)
(defconst gl-enemy-archery 110)
(defconst gl-enemy-cavalry 111)

;====================
(defconst gl-dlure 202)	;keeps track of the current luring stage
(defconst deer-id 203)	;stores chosen deer's ID as a way of keeping track of it
(defconst point-x 204)	;generic point goals 
(defconst point-y 205)
(defconst object-x 206)	;generic point goals
(defconst object-y 207)
(defconst goal 208)
(defconst TimeBeforeDeerLuring 150)	;seconds that must pass before starting to lure deer
(defconst TimeToStopDeerLuring 300)	;after this time luring will be disabled
(defconst DistanceToShootDeer 4)	;distance from TC to shoot deer
(defconst VillsToShootDeer 3)	;controls how many will shoot the deer
(defconst scout-unit scout-cavalry-line)	;which unit to push the deer with--change to eagle-warrior-line for meso civs
(defconst gl-distance-bottom-TC-corner 213)
(defconst gl-distance-upper-TC-corner 214)
(defconst gl-distance-left-TC-corner 215)
(defconst gl-distance-right-TC-corner 216)
(defconst gl-deer-found 217)
(defconst gl-tc-left-corner-x 218)
(defconst gl-tc-left-corner-y 219)
(defconst gl-tc-right-corner-x 220)
(defconst gl-tc-right-corner-y 221)
(defconst gl-tc-bottom-corner-x 222)
(defconst gl-tc-bottom-corner-y 223)
(defconst gl-tc-upper-corner-x 224)
(defconst gl-tc-upper-corner-y 225)
(defconst gl-deer-destination-x 226)
(defconst gl-deer-destination-y 227)
(defconst gl-temp-deer-destination-x 228)
(defconst gl-temp-deer-destination-y 229)
(defconst gl-deer-shot 231)
(defconst gl-temp 232)
(defconst gl-food-amount 233)
(defconst gl-selected-villager 234)
(defconst gl-total-food-amount 235)

(defconst closest-enemy-goal 236)
;endregion

;=================================================================

;region Villager escrow
(defrule
	(current-age >= feudal-age)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "TC")
)

(defrule
	(current-age >= feudal-age)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-train-count gl-temp-one)
	(up-get-fact escrow-amount food gl-temp-two)
	;(chat-to-all "Training and escrow value")
)

(defrule
	(current-age >= feudal-age)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 2)
	(unit-type-count-total villager < 50)
	(up-compare-goal gl-temp-two < 50)
=>
	(up-modify-escrow food c:= 50)
	;(chat-to-all "Setting escrow")
)

(defrule
	(current-age >= feudal-age)
	(up-set-target-object search-local c: 0)
	(or	(up-compare-goal gl-temp-one > 1)
		(unit-type-count-total villager >= 50))
	;(up-compare-goal gl-temp-two >= 50)
=>
	(release-escrow food)
	;(chat-to-all "Releasing escrow")
)

(defrule
	(current-age >= castle-age)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 2)
	(unit-type-count-total villager < 80)
	(up-compare-goal gl-temp-two < 50)
=>
	(up-modify-escrow food c:= 50)
	;(chat-to-all "Setting escrow")
)

(defrule
	(current-age >= castle-age)
	(up-set-target-object search-local c: 0)
	(or	(up-compare-goal gl-temp-one > 1)
		(unit-type-count-total villager >= 80))
	;(up-compare-goal gl-temp-two >= 50)
=>
	(release-escrow food)
	;(chat-to-all "Releasing escrow")
)
;endregion

;region Deer ID loop
(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-full-reset-search)
	(up-get-point position-map-size gl-temp-one);map size
	(up-modify-goal gl-temp-one g:* gl-temp-one);map size squared
	(up-modify-goal gl-temp-two c:= 0);indexer value
	;(up-chat-data-to-player my-player-number "Indexer = %d " g: gl-temp-two)
)

(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
	(up-set-target-by-id g: gl-temp-two)
=>
	(up-get-object-data object-data-class gl-temp-three)
)

(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
	(up-set-target-by-id g: gl-temp-two)
	(up-compare-goal gl-temp-three c:== prey-animal-class)
=>
	(up-add-object-by-id search-remote g: gl-temp-two)
	;(up-chat-data-to-player my-player-number "ID = %d " g: gl-temp-two)
)

(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
=>
	(up-modify-goal gl-temp-two c:+ 1)
	(up-jump-rule -3)
)

(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:== gl-temp-one)
=>
	(up-set-target-point gl-starting-tc-x)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Remote = %d " g: gl-remote-total)
)

(defrule
	(game-time == 10)
	(up-compare-goal gl-third-deer-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:== gl-temp-one)
=>
	(up-set-target-object search-remote c: 0)
	(up-get-object-data object-data-id gl-first-deer-id)
	(up-set-target-object search-remote c: 1)
	(up-get-object-data object-data-id gl-second-deer-id)
	(up-set-target-object search-remote c: 2)
	(up-get-object-data object-data-id gl-third-deer-id)
	;(up-chat-data-to-player my-player-number "Deer one = %d " g: gl-first-deer-id)
	;(up-chat-data-to-player my-player-number "Deer two = %d " g: gl-second-deer-id)
	;(up-chat-data-to-player my-player-number "Deer three = %d " g: gl-third-deer-id)
	;(up-chat-data-to-player my-player-number "Map size squared = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Indexer = %d " g: gl-temp-two)
)
;endregion

;region Boar ID loop
(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-full-reset-search)
	(up-get-point position-map-size gl-temp-one);map size
	(up-modify-goal gl-temp-one g:* gl-temp-one);map size squared
	(up-modify-goal gl-temp-two c:= 0);indexer value
	;(up-chat-data-to-player my-player-number "Indexer = %d " g: gl-temp-two)
)

(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
	(up-set-target-by-id g: gl-temp-two)
=>
	(up-get-object-data object-data-class gl-temp-three)
)

(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
	(up-set-target-by-id g: gl-temp-two)
	(up-compare-goal gl-temp-three c:== predator-animal-class)
=>
	(up-add-object-by-id search-remote g: gl-temp-two)
	;(up-chat-data-to-player my-player-number "ID = %d " g: gl-temp-two)
)

(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:< gl-temp-one)
=>
	(up-modify-goal gl-temp-two c:+ 1)
	(up-jump-rule -3)
)

(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:== gl-temp-one)
=>
	(up-set-target-point gl-starting-tc-x)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Remote = %d " g: gl-remote-total)
)

(defrule
	(game-time == 11)
	(up-compare-goal gl-first-boar-id < 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-two g:== gl-temp-one)
=>
	(up-set-target-object search-remote c: 0)
	(up-get-object-data object-data-id gl-first-boar-id)
	(up-set-target-object search-remote c: 1)
	(up-get-object-data object-data-id gl-second-boar-id)
	;(up-chat-data-to-player my-player-number "Boar one = %d " g: gl-first-boar-id)
	;(up-chat-data-to-player my-player-number "Boar two = %d " g: gl-second-boar-id)
	;(up-chat-data-to-player my-player-number "Map size squared = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Indexer = %d " g: gl-temp-two)
)
;endregion

(load "Sun Tzu 0.2a\Buildings")

;<<< Timers >>>
;region Three seconds timer
(defrule
	(true)
=>
	(enable-timer three-second-timer 3)
	;(chat-to-all "Timer start")
	(disable-self)
)
;endregion

;region Ten seconds timer
(defrule
	(current-age >= feudal-age)
=>
	(enable-timer ten-seconds-timer 10)
	;(chat-to-all "Timer start")
	(disable-self)
)
;endregion

;region One minute timer
(defrule
	(current-age >= feudal-age)
=>
	(enable-timer one-minute-timer 60)
	;(chat-to-all "Timer start")
	(disable-self)
)
;endregion



;region Bandit's targeting
#load-if-defined UP-POCKET-POSITION

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number <= 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(up-allied-sn any-ally sn-target-player-number s:== sn-target-player-number)
=>
(up-modify-sn sn-target-player-number c:= 0)
)

#end-if

(defrule
(or(not(players-stance target-player enemy))
(or(not(player-in-game target-player))
(strategic-number sn-target-player-number <= 0)))
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

(defrule
(or(not(players-stance target-player enemy))
(not(player-in-game target-player)))
(strategic-number sn-target-player-number > 0)
=>
(up-find-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-find-next-player enemy find-closest closest-enemy-goal)
(up-modify-sn sn-target-player-number g:= closest-enemy-goal)
)

;(defrule
;(true)
;=>
;(up-chat-data-to-all "Target %d" s: sn-target-player-number)
;)
;endregion



;region Attack if >= 10 combined army
(defrule
	(or	(unit-type-count skirmisher-line >= 10)
		(or	(unit-type-count militia-line >= 10)
			(or	(unit-type-count spearman-line >= 10)
				(or	(and	(unit-type-count skirmisher-line >= 5)
							(unit-type-count spearman-line >= 5))
					(or	(and	(unit-type-count skirmisher-line >= 5)
								(unit-type-count militia-line >= 5))
						(and	(unit-type-count spearman-line >= 5)
								(unit-type-count militia-line >= 5)))))))
	(timer-triggered one-minute-timer)
=>
	;(up-modify-sn sn-focus-player-number g:= gl-enemy-number)
	(attack-now)
)
;endregion

;<<< Strategic Numbers

;region Dark Age SNs - some were disabled
(defrule
	(true)
=>
	(up-modify-sn sn-enable-new-building-system c:= 1)
	(up-modify-sn sn-disable-villager-garrison c:= 2)
	(set-strategic-number sn-enable-training-queue 1)
	(set-strategic-number sn-cap-civilian-explorers 0)
	(set-strategic-number sn-total-number-explorers 1)
	(set-strategic-number sn-percent-civilian-explorers 0)
	(set-strategic-number sn-number-explore-groups 1)
	(up-modify-sn sn-enable-boar-hunting c:= 2);was disabled but rarely caused villagers to go hunt deer long-distance
	;(up-modify-sn sn-minimum-boar-lure-group-size c:= 0)
	;(up-modify-sn sn-minimum-number-hunters c:= 0)
	(set-strategic-number sn-cap-civilian-builders 5)
	;(set-strategic-number sn-maximum-wood-drop-distance 5)
	(set-strategic-number sn-enable-research-queue 1)
	(set-strategic-number sn-allow-adjacent-dropsites 1)
	(set-strategic-number sn-livestock-to-town-center 1)
	(set-strategic-number sn-maximum-food-drop-distance 2)
	(up-modify-sn sn-maximum-hunt-drop-distance c:= 4)
	(set-strategic-number sn-consecutive-idle-unit-limit 1)
	(disable-self)
)
;endregion

;region Feudal Age SNs
(defrule
	(current-age == feudal-age)
=>
	(up-modify-sn sn-disable-builder-assistance c:= 1)
)
;endregion

;region Castle Age SNs
(defrule
	(up-research-status c: castle-age >= research-pending)
=>
	(set-strategic-number sn-food-gatherer-percentage 50)
	(set-strategic-number sn-wood-gatherer-percentage 40)
	(set-strategic-number sn-gold-gatherer-percentage 10)
	(disable-self)
)
;endregion

;region Starting goal values
(defrule
	(true)
=>
	(up-modify-goal gl-scouting-direction c:= 0)
	(up-modify-goal gl-scouting-goal c:= 0)
	(disable-self)
)
;endregion

;region TC x/y coordinates
(defrule
	(true)
=>
	(up-get-point position-self gl-starting-tc-x)
	(disable-self)
)
;endregion

;region Finding TC x/y position and its four corners (for tree checks)
(defrule
	(true)
=>
	(up-get-point position-self gl-starting-tc-x) ; Finding and saving starting TC x/y coordinates
	(disable-self)
)

(defrule
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-modify-goal gl-tc-bottom-corner-x g:= gl-starting-tc-x)
	(up-modify-goal gl-tc-bottom-corner-x c:- 3)
	(up-modify-goal gl-tc-bottom-corner-y g:= gl-starting-tc-y)
	(up-modify-goal gl-tc-bottom-corner-y c:+ 3)
	(up-modify-goal gl-tc-left-corner-x g:= gl-tc-bottom-corner-x)
	(up-modify-goal gl-tc-left-corner-y g:= gl-tc-bottom-corner-y)
	(up-modify-goal gl-tc-left-corner-y c:- 6)
	(up-modify-goal gl-tc-right-corner-x g:= gl-tc-bottom-corner-x)
	(up-modify-goal gl-tc-right-corner-x c:+ 6)
	(up-modify-goal gl-tc-right-corner-y g:= gl-tc-bottom-corner-y)
	(up-modify-goal gl-tc-upper-corner-x  g:= gl-starting-tc-x)
	(up-modify-goal gl-tc-upper-corner-x c:+ 3)
	(up-modify-goal gl-tc-upper-corner-y g:= gl-starting-tc-y)
	(up-modify-goal gl-tc-upper-corner-y c:- 3)
	(disable-self)
)
;endregion

;region Control groups - to be improved after DUC is added - reassigning to different control groups, post-Dark Age assinging

;region Check if any villager is garrisoned - it can ruin control groups
(defrule
	(true)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1) ; It works
	;(chat-to-all "One")
)

(defrule
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-garrison-count gl-temp-one) ; It works
	;(chat-to-all "Two")
)
;endregion

;region Villager 1-6 - Sheep (Group 1)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class <= 6)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 6) ; It works
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class <= 6)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-local-total <= 6)
	(up-group-size c: 1 g:< gl-local-total) ; It works
=>
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Two")
)
;endregion

;region Villager 7 - 1st boar lure (Group 4)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 7)
	(up-group-size c: 4 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 7)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 7)
	(up-group-size c: 4 c:< 1)
	(up-compare-goal gl-local-total == 7)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	;(up-remove-objects search-local object-data-group-flag == 2)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 7)
	(up-group-size c: 4 < 1)
	(up-compare-goal gl-local-total == 1)
=>
	(up-modify-group-flag 0 c: 4)
	(up-reset-group c: 4)
	(up-create-group 0 0 c: 4)
	(up-modify-group-flag 1 c: 4)
	(disable-self)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 8-9 - Lumber Camp wood (Group 2)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 7)
    (unit-type-count villager-class < 10)
	(up-group-size c: 2 c:< 2)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 9)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 7)
    (unit-type-count villager-class < 10)
	(up-group-size c: 2 c:< 2)
	(up-compare-goal gl-local-total > 7)
	(up-compare-goal gl-local-total < 10)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 7)
    (unit-type-count villager-class < 10)
	(up-group-size c: 2 g:< gl-local-total)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-local-total <= 2)
=>
	(up-modify-group-flag 0 c: 2)
	(up-reset-group c: 2)
	(up-create-group 0 0 c: 2)
	(up-modify-group-flag 1 c: 2)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 10 - 2nd boar lure - (Group 4) + Reassigning 1st boar lure to group 1
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 c:== 1)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 10)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 c:== 1)
	(up-compare-goal gl-local-total == 10)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	;(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 c:== 1)
	(up-compare-goal gl-local-total == 1)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-group-flag 0 c: 4)
	(up-reset-group c: 4)
	(up-create-group 0 0 c: 4)
	(up-modify-group-flag 1 c: 4)
	(disable-self)
	;(chat-to-all "Group two edited")
)

;===
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 1 c:< 7)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 10)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 1 c:< 7)
	(up-compare-goal gl-local-total == 10)
=>
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 1 c:< 7)
	(up-compare-goal gl-local-total == 7)
=>
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Group two edited")
)

;<<< Group 4 -> 1 change >>>
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 10)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 c:< 1)
	(up-compare-goal gl-local-total == 10)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-get-search-state gl-local-total)
	(disable-self)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 10)
	(up-group-size c: 4 < 1)
	(up-compare-goal gl-local-total == 1)
=>
	(up-modify-group-flag 0 c: 4)
	(up-reset-group c: 4)
	(up-create-group 0 0 c: 4)
	(up-modify-group-flag 1 c: 4)
	(disable-self)
	;(chat-to-all "Group two edited")
)

;endregion

;region Villager 11 - TC food (sheep, boar, deer) (Group 1)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 11)
	(up-group-size c: 1 c:< 9)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 11)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 11)
	(up-group-size c: 1 c:< 9)
	(up-compare-goal gl-local-total == 11)
=>
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Two")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 11)
	(up-group-size c: 1 c:< 9)
	(or	(up-compare-goal gl-local-total == 8)
		(up-compare-goal gl-local-total == 9))
=>
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Three")
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 12 - House + mill -> berries (Group 6)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 12)
	(up-group-size c: 6 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 12)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 12)
	(up-group-size c: 6 c:< 1)
	(up-compare-goal gl-local-total == 12)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 12)
	(up-group-size c: 6 c:< 1)
	(up-compare-goal gl-local-total == 1)
=>
	(up-modify-group-flag 0 c: 6)
	(up-reset-group c: 6)
	(up-create-group 0 0 c: 6)
	(up-modify-group-flag 1 c: 6)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 13 - Straggler wood (Group 5 -> Group 2 later)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 13)
	(up-group-size c: 5 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 13)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 13)
	(up-group-size c: 5 c:< 1)
	(up-compare-goal gl-local-total == 13)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class == 13)
	(up-group-size c: 5 c:< 1)
	(up-compare-goal gl-local-total == 1)
=>
	(up-modify-group-flag 0 c: 5)
	(up-reset-group c: 5)
	(up-create-group 0 0 c: 5)
	(up-modify-group-flag 1 c: 5)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 14-16 - TC food (sheep, boar, deer) (Group 1) - edit reassigning boar lures
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 13)
	(unit-type-count villager-class < 17)
	(up-group-size c: 1 c:< 12)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 16)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 13)
	(unit-type-count villager-class < 17)
	(up-group-size c: 1 c:< 12)
	(up-compare-goal gl-local-total > 13)
	(up-compare-goal gl-local-total < 17)
=>
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 13)
	(unit-type-count villager-class < 17)
	(up-group-size c: 1 g:< gl-local-total)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-local-total <= 12)
=>
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 17 - Berries (Group 6)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 16)
    (unit-type-count villager-class < 18)
	(up-group-size c: 6 c:< 2)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 17)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 16)
    (unit-type-count villager-class < 18)
	(up-group-size c: 6 c:< 2)
	(up-compare-goal gl-local-total > 16)
	(up-compare-goal gl-local-total < 18)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 16)
    (unit-type-count villager-class < 18)
	(up-group-size c: 6 g:< gl-local-total)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-local-total <= 2)
=>
	(up-modify-group-flag 0 c: 6)
	(up-reset-group c: 6)
	(up-create-group 0 0 c: 6)
	(up-modify-group-flag 1 c: 6)
	;(chat-to-all "Group two edited")
)
;endregion

;region Villager 18-19 - Straggler wood (Group 5 -> Group 2 later)
(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 17)
    (unit-type-count villager-class < 20)
	(up-group-size c: 5 c:< 5)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 19)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 17)
    (unit-type-count villager-class < 20)
	(up-group-size c: 5 c:< 5)
	(up-compare-goal gl-local-total > 17)
	(up-compare-goal gl-local-total < 20)
=>
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-compare-goal gl-temp-one < 1)
	(current-age == dark-age);(up-research-status c: feudal-age < research-pending)
    (unit-type-count villager-class > 17)
    (unit-type-count villager-class < 20)
	(up-group-size c: 5 g:< gl-local-total)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-local-total <= 3)
=>
	(up-modify-group-flag 0 c: 5)
	(up-reset-group c: 5)
	(up-create-group 0 0 c: 5)
	(up-modify-group-flag 1 c: 5)
	;(chat-to-all "Group two edited")
)
;endregion

;region Control group 3 - gold after Feudal Age research starts
(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-group-size c: 3 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1) ; It works
	;(chat-to-all "One")
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-group-size c: 3 c:< 1)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-garrison-count gl-temp-one) ; It works
	;(chat-to-all "Two")
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 3 c:< 1)
=>
	;(chat-to-all "Time to mine some gold")
	(up-full-reset-search)
	(up-set-group search-local c: 1)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting group = %d " g: gl-local-total)
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 3 c:< 1)
	(up-set-target-object search-local c: 0)
=>
	(up-clean-search search-local object-data-precise-distance search-order-asc)
	(up-remove-objects search-local object-data-index > 7)
	;(up-remove-objects search-local object-data-hitpoints < 40)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After removing villagers = %d " g: gl-local-total)
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Group one recreated")
	;(up-remove-objects search-local object-data-index > 2)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting only three villagers = %d " g: gl-local-total)
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 3 c:< 1)
	(up-set-target-object search-local c: 0)
	;(unit-type-count villager-class == 19)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 19)
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
	(up-remove-objects search-local object-data-index > 2)
	;(up-chat-data-to-player my-player-number "After removing villagers = %d " g: gl-local-total)
	;(chat-to-all "Creating group three")
	(up-modify-group-flag 0 c: 3)
	(up-reset-group c: 3)
	(up-create-group 0 0 c: 3)
	(up-modify-group-flag 1 c: 3)
	;(up-remove-objects search-local object-data-index > 2)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting only three villagers = %d " g: gl-local-total)
)
;endregion

;region Post Feudal food drop - 2 more straggler tree lumberjacks
(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-group-size c: 5 c:<= 4)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1) ; It works
	;(chat-to-all "One")
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-group-size c: 5 c:<= 4)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-garrison-count gl-temp-one) ; It works
	;(chat-to-all "Two")
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 5 c:<= 4)
=>
	;(chat-to-all "Time to mine some gold")
	(up-full-reset-search)
	(up-set-group search-local c: 1)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting group = %d " g: gl-local-total)
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 5 c:<= 4)
	(up-set-target-object search-local c: 0)
=>
	;(up-clean-search search-local object-data-precise-distance search-order-asc);why?
	(up-clean-search search-local object-data-hitpoints search-order-asc)
	(up-remove-objects search-local object-data-index > 5)
	;(up-remove-objects search-local object-data-hitpoints < 40)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After removing villagers = %d " g: gl-local-total)
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Group one recreated")
	;(up-remove-objects search-local object-data-index > 2)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting only three villagers = %d " g: gl-local-total)
)

(defrule
	(or	(and(food-amount >= 500)
			(current-age == dark-age))
	(up-research-status c: feudal-age >= research-pending))
	(up-compare-goal gl-temp-one < 1)
	(up-group-size c: 5 c:<= 4)
	(up-set-target-object search-local c: 0)
=>
	(up-full-reset-search)
	(up-find-local c: villager-class c: 19)
	(up-remove-objects search-local object-data-group-flag == 1)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 3)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-remove-objects search-local object-data-group-flag == 6)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After removing villagers = %d " g: gl-local-total)
	;(chat-to-all "Creating group three")
	(up-modify-group-flag 0 c: 5)
	(up-reset-group c: 5)
	(up-create-group 0 0 c: 5)
	(up-modify-group-flag 1 c: 5)
	;(up-remove-objects search-local object-data-index > 2)
	;(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After selecting only three villagers = %d " g: gl-local-total)
)
;endregion

;endregion

;region Gold miners
(defrule
	(up-group-size c: 3 c:> 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 3)
	(up-remove-objects search-local object-data-type == gold-miner-male)
	(up-remove-objects search-local object-data-type == gold-miner-female)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Non-miners = %d " g: gl-local-total)
)

(defrule
	(up-group-size c: 3 c:> 0)
	(up-compare-goal gl-local-total > 0)
=>
	(up-filter-status c: status-resource c: list-active)
	(up-find-resource c: gold c: 11)
	(up-set-target-point gl-starting-tc-x)
	(up-clean-search search-remote object-data-precise-distance search-order-asc)
	;(chat-to-all "Retask one")
)

(defrule
	(up-group-size c: 3 c:> 0)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Retask two")
)
;endregion

;region Defining sheep gathering points
(defrule
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-modify-goal gl-sheep-tc-gathering-x g:= gl-starting-tc-x)
	(up-modify-goal gl-sheep-tc-gathering-x c:- 1)
	(up-modify-goal gl-sheep-tc-gathering-y g:= gl-starting-tc-y)
	(up-modify-goal gl-sheep-tc-gathering-y c:+ 1)
	(up-modify-goal gl-sheep-gathering-x g:= gl-starting-tc-x)
	(up-modify-goal gl-sheep-gathering-x c:+ 3)
	(up-modify-goal gl-sheep-gathering-y g:= gl-starting-tc-y)
	(up-modify-goal gl-sheep-gathering-y c:+ 3)
	(disable-self)
)
;endregion

;region Sending sheep to TC one at a time
(defrule
	(unit-type-count livestock-class c:>= 1)
=>
	(up-full-reset-search)
	(up-find-local c: livestock-class c: 10)
	(up-get-search-state gl-local-total)
)

(defrule
	(unit-type-count livestock-class c:>= 1)
	(up-compare-goal gl-local-total > 0)
=>
	(up-set-target-point gl-starting-tc-x)
	;(up-chat-data-to-player my-player-number "gl-starting-tc-x = %d " g: gl-starting-tc-x)
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
	(up-clean-search search-local object-data-precise-distance search-order-asc)
	;(up-clean-search search-local object-data-distance search-order-asc)
	;(up-set-target-object search-local c: 0)
	;(up-get-object-data object-data-id gl-villager-target-id)
	(up-remove-objects search-local object-data-group-flag == 8)
	(up-target-point gl-sheep-tc-gathering-x action-default -1 -1)
	(up-remove-objects search-local object-data-index < 1)
	(up-get-search-state gl-local-total)	
)

(defrule
	(unit-type-count livestock-class c:>= 2)
	(up-compare-goal gl-local-total > 0)
=>
	;(up-clean-search search-local object-data-precise-distance search-order-asc)
	(up-remove-objects search-local object-data-group-flag == 8)
	(up-target-point gl-sheep-gathering-x action-default -1 -1)
	;(up-chat-data-to-player my-player-number "Sheep found = %d " g: gl-local-total)
)

;endregion

;region Setting boars as gather point for TC, boar destination change, TC gather point reset after boar lure starts
(defrule
	(timer-triggered twenty-five-seconds-timer)
	;(dropsite-min-distance live-boar < 30)
	;(dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "One")
)

(defrule
	(timer-triggered twenty-five-seconds-timer)
	;(dropsite-min-distance live-boar < 30)
	;(dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-local c: 0)
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
=>
	;(set-strategic-number sn-focus-player-number 0)
	;(up-find-remote c: predator-animal-class c: 2)
	(up-add-object-by-id search-remote g: gl-first-boar-id)
	(up-add-object-by-id search-remote g: gl-second-boar-id)
	(up-remove-objects search-remote object-data-hitpoints < 1)
	(up-clean-search search-remote -1 search-order-asc)
	(up-modify-goal gl-temp-one c:= 0)
	;(chat-to-all "Two")
)

(defrule
	(timer-triggered twenty-five-seconds-timer)
	;(dropsite-min-distance live-boar < 30)
	;(dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-local c: 0)
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one < 2)
=>
	(up-get-object-data object-data-point-x gl-temp-two)
	(up-get-object-data object-data-point-y gl-temp-three)
	(up-set-target-point gl-temp-two)
	;(chat-to-all "Three")
)

(defrule
	(timer-triggered twenty-five-seconds-timer)
	;(dropsite-min-distance live-boar < 30)
	;(dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-local c: 0)
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one < 2)
	(up-point-explored gl-temp-two == explored-no)
=>
	(up-modify-goal gl-temp-one c:+ 1)
	;(chat-to-all "Jump")
	(up-jump-rule -2)
)

(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	;(dropsite-min-distance live-boar < 30)
	;(dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-local c: 0)
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one < 2)
	(up-point-explored gl-temp-two != explored-no)
=>
	(up-target-objects 1 action-gather -1 -1)
	;(chat-to-all "Four")
)

;Adjusting 1st boar destination
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(or (unit-type-count villager == 6)
		(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	;Boar x/y
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	;Left TC corner
	(up-modify-goal gl-temp-three g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-three c:- 2)
	(up-modify-goal gl-temp-four g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-four c:- 2)
	;Right TC corner
	(up-modify-goal gl-temp-five g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-five c:+ 2)
	(up-modify-goal gl-temp-six g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-six c:+ 2)
	(up-set-target-point gl-temp-three)
	;Distance between boar and left TC corner
	(up-get-point-distance gl-temp-one gl-temp-three gl-temp-seven)
	;Distance between boar and right TC corner
	(up-get-point-distance gl-temp-one gl-temp-five gl-temp-eight)
	;(chat-to-all "One")
)

;If boar is above TC - left
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-one g:>= gl-starting-tc-x)
	(up-compare-goal gl-temp-two g:<= gl-starting-tc-y)
	(up-compare-goal gl-temp-seven g:<= gl-temp-eight)
	(or (unit-type-count villager == 6)
	(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	;(set-strategic-number sn-boar-lure-destination 12) center
	;(set-strategic-number sn-boar-lure-destination 13) center
	(set-strategic-number sn-boar-lure-destination 8)
	;(chat-to-all "Two - above TC - left")
)

;If boar is above TC - right
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-one g:>= gl-starting-tc-x)
	(up-compare-goal gl-temp-two g:<= gl-starting-tc-y)
	(up-compare-goal gl-temp-seven g:> gl-temp-eight)
	(or (unit-type-count villager == 6)
	(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	;(set-strategic-number sn-boar-lure-destination 12) center
	;(set-strategic-number sn-boar-lure-destination 13) center
	(set-strategic-number sn-boar-lure-destination 2)
	;(chat-to-all "Two - above TC - right")
)

;If boar is on the left of TC
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-one g:< gl-starting-tc-x)
	(up-compare-goal gl-temp-two g:< gl-starting-tc-y)
	(or (unit-type-count villager == 6)
	(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	(set-strategic-number sn-boar-lure-destination 1)
	;(chat-to-all "Three - to the left of TC")
)

;If boar is on the right of TC
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-one g:> gl-starting-tc-x)
	(up-compare-goal gl-temp-two g:> gl-starting-tc-y)
	(or (unit-type-count villager == 6)
	(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	(set-strategic-number sn-boar-lure-destination 7)
	;(chat-to-all "Four - to the right of TC")
)

;If boar is below TC
(defrule
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-temp-one < 2)
	; (dropsite-min-distance live-boar < 30)
	; (dropsite-min-distance live-boar > 0)
	(or	(up-compare-goal gl-first-boar-id > -1)
		(up-compare-goal gl-second-boar-id > -1))
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-one g:<= gl-starting-tc-x)
	(up-compare-goal gl-temp-two g:>= gl-starting-tc-y)
	(or (unit-type-count villager == 6)
	(unit-type-count villager == 9))
	(up-compare-goal gl-temp-one < 2)
=>
	(set-strategic-number sn-boar-lure-destination 0)
	;(chat-to-all "Five - below TC")
)

(defrule
	(or (unit-type-count villager == 7)
	(unit-type-count villager == 10))
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "Four")
)

(defrule
	(unit-type-count villager == 7)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-goal gl-temp-one c:= -1)
	(up-modify-goal gl-temp-two c:= -1)
	(up-target-point gl-temp-one action-gather -1 -1)
	;(chat-to-all "Five")
	(disable-self)
)

(defrule
	(unit-type-count villager == 10)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-goal gl-temp-one c:= -1)
	(up-modify-goal gl-temp-two c:= -1)
	(up-target-point gl-temp-one action-gather -1 -1)
	;(chat-to-all "Five")
	(disable-self)
)
;endregion

;region Safety boar lure if gather point is changed to deer right before the villager is made
(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
=>
	(up-full-reset-search)
	(up-add-object-by-id search-remote g: gl-first-boar-id)
	(up-add-object-by-id search-remote g: gl-second-boar-id)
	(up-modify-goal gl-temp-one c:= 0)
	(up-set-group search-local c: 4)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
=>
	(up-get-object-data object-data-hitpoints gl-temp-two)
	;(chat-to-all "Two")
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two < 1)
=>
	(up-modify-goal gl-temp-one c:+ 1)
	;(chat-to-all "Three Jump")
	(up-jump-rule -2)
	;(chat-to-all "Three - first boar is dead")
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
=>
	(up-get-object-data object-data-point-x gl-temp-three)
	(up-get-object-data object-data-point-y gl-temp-four)
	;(chat-to-all "Four")
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
	(up-point-explored gl-temp-three == explored-no)
=>
	(up-modify-goal gl-temp-one c:+ 1)
	;(chat-to-all "Five Jump")
	(up-jump-rule -4)
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
	(up-point-explored gl-temp-three != explored-no)
=>
	(up-get-object-data object-data-target gl-temp-five)
	(up-set-target-object search-local g: gl-temp-one)
	(up-get-object-data object-data-target gl-temp-six)
	;(chat-to-all "Six")
	;(up-chat-data-to-player my-player-number "Boar's target = %d " g: gl-temp-five)
	;(up-chat-data-to-player my-player-number "Villager's target = %d " g: gl-temp-six)
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
	(up-point-explored gl-temp-three != explored-no)
	(up-compare-goal gl-temp-five == villager-class)
=>
	;(chat-to-all "Seven - Boar is chasing a villager");move villager from group 4 to group 1
	(up-full-reset-search)
	(up-modify-group-flag 0 c: 4)
	(up-reset-group c: 4)
	(up-create-group 0 0 c: 4)
	(up-modify-group-flag 1 c: 4)
	(up-find-local c: villager-class c: 40)
	(up-remove-objects search-local object-data-group-flag == 2)
	(up-remove-objects search-local object-data-group-flag == 3)
	(up-remove-objects search-local object-data-group-flag == 5)
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
)

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
	(up-point-explored gl-temp-three != explored-no)
	(up-compare-goal gl-temp-five != villager-class)
	(up-compare-goal gl-temp-six == predator-animal-class)
=>
	(up-modify-goal gl-temp-one c:+ 1)
	;(chat-to-all "Eight Jump")
	(up-jump-rule -7)
)
	

(defrule
	(up-group-size c: 4 > 0)
	(up-compare-goal gl-first-boar-id > -1)
	(up-compare-goal gl-second-boar-id > -1)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
	(up-compare-goal gl-temp-two > 0)
	(up-point-explored gl-temp-three != explored-no)
	(up-compare-goal gl-temp-five != villager-class)
	(up-compare-goal gl-temp-six != predator-animal-class)
=>
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Nine - Target")
)
;endregion

;region Food drop for Feudal Age and changing SNs - to be simplified and improved - up-jump-rule "Attacking boar" and "Keeping group 1 doing their job"
(defrule
	(unit-type-count villager > 17)
	(unit-type-count villager < 20)
=>
	(up-full-reset-search)
	(up-modify-sn sn-focus-player-number c:= my-player-number)
	;(up-modify-goal gl-temp-one c:= 0);gl-food-amount
	(up-get-fact food-amount 0 gl-temp-one);gl-food-amount
)

(defrule
	(unit-type-count villager > 17)
	(unit-type-count villager < 20)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 1)
	(up-get-search-state gl-local-total)
	(up-modify-goal gl-temp-two c:= 0);gl-selected-villager
)

(defrule
	(unit-type-count villager > 17)
	(unit-type-count villager < 20)
	(up-compare-goal gl-local-total c:> 0)
	(up-compare-goal gl-temp-two g:< gl-local-total);gl-selected-villager
=>
	(up-set-target-object search-local g: gl-temp-two);gl-selected-villager
	(up-get-object-data object-data-carry gl-temp-three);gl-total-food-amount
)

(defrule
	(unit-type-count villager > 17)
	(unit-type-count villager < 20)
	(up-compare-goal gl-local-total c:> 0)
	(up-compare-goal gl-temp-two g:< gl-local-total);gl-selected-villager
	(up-modify-goal gl-temp-one g:+ gl-temp-three);gl-food-amount and gl-total-food-amount
	(up-modify-goal gl-temp-two c:+ 1)
=>
	(up-jump-rule -2)
)

(defrule
	(unit-type-count villager > 17)
	(unit-type-count villager < 20)
	(up-compare-goal gl-temp-one c:>= 500);gl-food-amount
=>
    (up-find-remote c: town-center c: 1)
    (up-set-target-object search-remote c: 0)
    (up-target-objects 1 action-garrison -1 -1)
	(up-jump-rule 14)
	(disable-self)
)
;endregion

;region Attacking boar - (dropsite-min-distance live-boar < 6) to be replaced? - up-jump-rule "Keeping group 1 doing their job"
(defrule
	(unit-type-count villager >= 6)
	(dropsite-min-distance live-boar < 6)
	(dropsite-min-distance live-boar >= 0)
=>
	(up-full-reset-search)
	(up-set-target-point gl-starting-tc-x)
    (set-strategic-number sn-focus-player-number 0)
	(up-filter-distance c: -1 c: 7)
	(up-find-remote c: predator-animal-class c: 1)
	;(chat-to-all "Boar check - One")
)

(defrule
	(unit-type-count villager >= 6)
	(dropsite-min-distance live-boar < 6)
	(dropsite-min-distance live-boar >= 0)
	(up-set-target-object search-remote c: 0)
=>
	;Precise boar x/y
	(up-get-object-data object-data-precise-x gl-temp-one)
	(up-get-object-data object-data-precise-y gl-temp-two)
	;(up-chat-data-to-player my-player-number "Boar X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Boar Y = %d " g: gl-temp-two)
	;Left x edge
	(up-modify-goal gl-temp-three g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-three c:* 100)
	(up-modify-goal gl-temp-three c:- 250)
	;Upper y edge
	(up-modify-goal gl-temp-four g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-four c:* 100)
	(up-modify-goal gl-temp-four c:- 250)
	;Right x edge
	(up-modify-goal gl-temp-five g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-five c:* 100)
	(up-modify-goal gl-temp-five c:+ 250)
	;Lower y edge
	(up-modify-goal gl-temp-six g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-six c:* 100)
	(up-modify-goal gl-temp-six c:+ 250)
	;TC X precise
	(up-modify-goal gl-temp-seven g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-seven c:* 100)
	;TC Y precise
	(up-modify-goal gl-temp-eight g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-eight c:* 100)
	;(chat-to-all "Boar check - Two")
)

(defrule
	(unit-type-count villager >= 6)
	(dropsite-min-distance live-boar < 6)
	(dropsite-min-distance live-boar >= 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-temp-one g:>= gl-temp-three)
	(up-compare-goal gl-temp-one g:<= gl-temp-seven)
	(up-compare-goal gl-temp-two g:>= gl-temp-four)
	(up-compare-goal gl-temp-two g:<= gl-temp-six)
=>
	;(up-chat-data-to-player my-player-number "TC X = %d " g: gl-temp-three)
	;(up-chat-data-to-player my-player-number "TC Y = %d " g: gl-temp-four)
	(up-set-group search-local c: 1)
	(up-remove-objects search-local object-data-hitpoints < 25)
	;(up-remove-objects search-local object-data-type == builder-male)
	;(up-remove-objects search-local object-data-type == builder-female)
	(up-clean-search search-local -1 search-order-asc)
	(up-remove-objects search-local object-data-index > 6)
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Two - left")
)

(defrule
	(unit-type-count villager >= 6)
	(dropsite-min-distance live-boar < 6)
	(dropsite-min-distance live-boar >= 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-temp-one g:> gl-temp-seven)
	(up-compare-goal gl-temp-one g:<= gl-temp-five)
	(up-compare-goal gl-temp-two g:>= gl-temp-eight)
	(up-compare-goal gl-temp-two g:<= gl-temp-six)
=>
	;(up-chat-data-to-player my-player-number "TC X = %d " g: gl-temp-three)
	;(up-chat-data-to-player my-player-number "TC Y = %d " g: gl-temp-four)
	(up-set-group search-local c: 1)
	(up-remove-objects search-local object-data-hitpoints < 25)
	;(up-remove-objects search-local object-data-type == builder-male)
	;(up-remove-objects search-local object-data-type == builder-female)
	(up-clean-search search-local -1 search-order-asc)
	(up-remove-objects search-local object-data-index > 6)
	(up-target-objects 1 action-default -1 -1)
	(up-jump-rule 10)
	;(chat-to-all "Two - right")
)
;endregion

;region Keeping group 1 doing their job - food :)
;Boar/deer food left check

;Adding boars/deer to remote list and removing unnecessary ones
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-full-reset-search)
	(up-set-target-point gl-starting-tc-x)
	(up-add-object-by-id search-remote g: gl-first-deer-id)
	(up-add-object-by-id search-remote g: gl-second-deer-id)
	(up-add-object-by-id search-remote g: gl-third-deer-id)
	(up-add-object-by-id search-remote g: gl-first-boar-id)
	(up-add-object-by-id search-remote g: gl-second-boar-id)
	(up-remove-objects search-remote object-data-hitpoints > 0)
	(up-remove-objects search-remote object-data-carry < 1)
	(up-remove-objects search-remote object-data-distance > 10)
	(up-get-search-state gl-local-total)
	; (up-modify-goal gl-temp-one c:= 0)
	; (up-modify-goal gl-temp-two c:= 0)
	; (up-modify-goal gl-temp-three c:= 0)
	; (up-modify-goal gl-temp-four c:= 0)
	; (up-modify-goal gl-temp-five c:= 0)
	; (up-modify-goal gl-temp-six c:= 0)
	; (up-modify-goal gl-temp-seven c:= 0)
	; (up-modify-goal gl-temp-eight c:= 0)
	;(chat-to-all "Looking for animals")
	;(up-chat-data-to-player my-player-number "Animals found = %d " g: gl-remote-total)
)

;Sorting remote list by food left, setting list indexers to 0
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-remote c: 0)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	;(up-compare-goal gl-temp-one g:< gl-local-total)
	;(up-compare-goal gl-temp-two g:< gl-remote-total)
=>
	(up-set-group search-local c: 1)
	(up-clean-search search-remote object-data-carry search-order-asc)
	(up-get-search-state gl-local-total)
	(up-modify-goal gl-temp-one c:= 0);local list indexer
	(up-modify-goal gl-temp-two c:= 0);remote list indexer
	;(chat-to-all "Sorting")
)

;Getting lowest food left object ID
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-remote g: gl-temp-two)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
=>
	(up-get-object-data object-data-id gl-temp-three);ID of currently selected animal
	(up-get-object-data object-data-class gl-temp-four);Class of currently selected animal
	;(chat-to-all "Getting ID")
)

;Setting max villagers for current target to 7 if that's a boar
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-remote g: gl-temp-two)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-four c:== predator-animal-class)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
=>
	(up-modify-goal gl-temp-five c:= 7);Number of villagers needed
	;(chat-to-all "It's a boar")
)

;Setting max villagers for current target to 5 if that's a deer
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-remote g: gl-temp-two)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-four c:== prey-animal-class)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
=>
	(up-modify-goal gl-temp-five c:= 5);Number of villagers needed
	;(chat-to-all "It's a deer")
)

;Getting ID of villager's target
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-local g: gl-temp-one)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-five > 0)
=>
	(up-get-object-data object-data-target-id gl-temp-six);ID of villager's target
	;(chat-to-all "Villager check")
)

;Checking if villagers's target is the currently preferred animal, if yes, removing that villager from local list and lowering villagers needed to gather
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-local g: gl-temp-one)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-five > 0)
	(up-compare-goal gl-temp-six g:== gl-temp-three)
=>
	(up-remove-objects search-local object-data-index g:== gl-temp-one)
	(up-modify-goal gl-temp-five c:- 1)
	;(up-modify-goal gl-temp-one c:+ 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "First Jump")
	(up-jump-rule -2)
)

;Checking if villagers's target is the currently preferred animal, if not, increasing local search index
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	(up-set-target-object search-local g: gl-temp-one)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-compare-goal gl-temp-two g:< gl-remote-total)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-five > 0)
	(up-compare-goal gl-temp-six g:!= gl-temp-three)
=>
	;(up-remove-objects search-local object-data-index g:!= gl-temp-one)
	;(up-modify-goal gl-temp-five c:- 1)
	(up-modify-goal gl-temp-one c:+ 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Second Jump")
	(up-jump-rule -3)
)

;If enough villagers are assigned to the animal, select next animal
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	;(up-set-target-object search-local g: gl-temp-one)
	(up-compare-goal gl-temp-one > 0)
	;(up-compare-goal gl-temp-one g:< gl-local-total)
	;(up-compare-goal gl-temp-two g:< gl-remote-total)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-five == 0)
	;(up-set-target-object search-remote g: gl-temp-two)
	;(up-compare-goal gl-temp-six g:== gl-temp-three)
=>
	(up-modify-goal gl-temp-one c:= 0)
	(up-modify-goal gl-temp-two c:+ 1)
	;(chat-to-all "Third Jump")
	(up-jump-rule -7)
)

;If not enough villagers are assigned to the animal, retask closest villagers
(defrule
	(up-group-size c: 1 > 0)
	(unit-type-count villager-class > 6)
	;(up-compare-goal gl-temp-one g:< gl-local-total)
	;(timer-triggered three-second-timer)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one g:== gl-local-total)
	(up-compare-goal gl-local-total > 0)
	;(up-compare-goal gl-temp-two g:< gl-remote-total)
	;(up-set-target-object search-local g: gl-temp-one)
	(up-compare-goal gl-temp-five > 0)
	(up-set-target-object search-remote g: gl-temp-two)
	;(up-compare-goal gl-temp-six g:== gl-temp-three)
=>
	;(up-modify-goal gl-temp-one c:= 0)
	(up-get-object-data object-data-point-x gl-temp-seven)
	(up-get-object-data object-data-point-y gl-temp-eight)
	(up-set-target-point gl-temp-seven)
	(up-clean-search search-local object-data-precise-distance search-order-asc)
	(up-remove-objects search-local object-data-index g:>= gl-temp-five)
	(up-set-target-object search-remote g:= gl-temp-two)
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Retask")
	;(up-jump-rule 1)
)

;endregion

;region Retasking shepherds to boar/deer
(defrule
	(up-remaining-boar-amount > 0)
	(up-remaining-boar-amount < 400)
=>
	(up-full-reset-search)
	(up-find-local c: shepherd-male c: 5)
	(up-find-local c: shepherd-female c: 5)
	(up-remove-objects search-local object-data-group-flag != 1)
	(up-modify-sn sn-focus-player-number c:= 0)
	(up-filter-status c: status-gather c: list-inactive)
	(up-set-target-point gl-starting-tc-x)
	(up-filter-distance c: -1 c: 10)
	(up-find-status-remote c: predator-animal-class c: 1)
	(up-find-status-remote c: prey-animal-class c: 2)
	(up-get-search-state gl-local-total)
)

(defrule
	(up-remaining-boar-amount > 0)
	(up-remaining-boar-amount < 400)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-target-objects 1 action-default -1 -1)
)
;endregion

;region Keeping shepherds targeting the same sheep
(defrule
	(current-age == dark-age)
	(unit-type-count villager > 3)
	(up-compare-goal gl-starting-tc-x > -1)
=>
	(up-full-reset-search)
	(up-modify-sn sn-focus-player-number c:= 0)
	(up-set-target-point gl-starting-tc-x)
	(up-filter-distance c: -1 c: 10)
	(up-filter-status c: status-gather c: list-inactive)
	;(up-find-status-remote c: predator-animal-class c: 2)
	;(up-find-status-remote c: prey-animal-class c: 2)
	(up-find-status-remote c: livestock-class c: 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
	;(up-chat-data-to-player my-player-number "gl-remote-total = %d " g: gl-remote-total)
)

(defrule
	(current-age == dark-age)
	(unit-type-count villager > 3)
	(up-compare-goal gl-starting-tc-x > -1)
	(up-compare-goal gl-remote-total == 1)
	(up-set-target-object search-remote c: 0)
=>
	(up-get-object-data object-data-id gl-temp-one)
	(up-set-group search-local c: 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Two")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
)

(defrule
	(current-age == dark-age)
	(unit-type-count villager > 3)
	(up-compare-goal gl-starting-tc-x > -1)
	(up-compare-goal gl-remote-total == 1)
	(up-compare-goal gl-local-total > 0)
=>
	(up-reset-filters)
	(up-remove-objects search-local object-data-target-id g:== gl-temp-one)
	(up-remove-objects search-local object-data-type == hunter-male)
	(up-remove-objects search-local object-data-type == hunter-female)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Three")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
)

(defrule
	(current-age == dark-age)
	(unit-type-count villager > 3)
	(up-compare-goal gl-starting-tc-x > -1)
	(up-compare-goal gl-remote-total == 1)
	(up-compare-goal gl-local-total > 0)
=>
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Four")
)

;region Retasking wrong lumber camp builder back to food
(defrule
	(building-type-count lumber-camp < 1)
	(building-type-count-total lumber-camp > 0)
	(up-group-size c: 1 c:> 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 1)
	(up-remove-objects search-local object-data-type == shepherd-male)
	(up-remove-objects search-local object-data-type == shepherd-female)
	(up-remove-objects search-local object-data-type == hunter-male)
	(up-remove-objects search-local object-data-type == hunter-female)
	(up-get-search-state gl-local-total)
)

(defrule
	(building-type-count lumber-camp < 1)
	(building-type-count-total lumber-camp > 0)
	(up-group-size c: 1 c:> 0)
	(up-compare-goal gl-local-total > 0)
=>
	(up-modify-sn sn-focus-player-number c:= 0)
	(up-filter-status c: status-gather c: list-inactive)
	(up-set-target-point gl-starting-tc-x)
	(up-filter-distance c: -1 c: 10)
	(up-find-status-remote c: predator-animal-class c: 2)
	(up-find-status-remote c: prey-animal-class c: 2)
	(up-clean-search search-remote object-data-carry search-order-asc)
	(up-find-status-remote c: livestock-class c: 2)
	(up-filter-distance c: -1 c: 5)
	(up-filter-status c: status-gather c: list-active)
	(up-find-status-remote c: livestock-class c: 2)
	(up-get-search-state gl-local-total)
)

(defrule
	(building-type-count lumber-camp < 1)
	(building-type-count-total lumber-camp > 0)
	(up-group-size c: 1 c:> 0)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-remote-total > 0)
=>
	(up-target-objects 0 action-default -1 -1)
	;(chat-to-all "Back to food")
)
;endregion

;region Berries - retasking group 6 to berries

;Check if any villager from control group 6 changed task
(defrule
	(up-group-size c: 6 c:> 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 6)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-remove-objects search-local object-data-type == forager-male)
	(up-remove-objects search-local object-data-type == forager-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Berry one")
)

;Looking for berries so they can be retasked
(defrule
	(up-group-size c: 6 c:> 0)
	(up-compare-goal gl-local-total > 0)
=>
	(up-set-target-point gl-starting-tc-x)
	(set-strategic-number sn-focus-player-number 0)
	(up-filter-distance c: -1 c: 20)
	(up-find-remote c: forage-class c: 6)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Berry two")
)

;Retasking
(defrule
	(up-group-size c: 6 c:> 0)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-clean-search search-remote object-data-distance search-order-asc)
	(up-set-target-object search-remote c: 0)
	(up-target-objects 1 action-default -1 -1)
	(up-jump-rule 8)
	;(chat-to-all "Berry three")
)

;Berry gatherers -> Control group 1 once berries are gone
;Looking for berries
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
=>
	(up-set-target-point gl-starting-tc-x)
	(set-strategic-number sn-focus-player-number 0)
	(up-filter-distance c: -1 c: 20)
	(up-find-remote c: forage-class c: 6)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Berry bushes left = %d " g: gl-remote-total)
)

;If berries are found, up-jump-rule
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-remote-total > 0)
=>
	(up-jump-rule 6)
)

;If no berries are found, group 1 into remote, group 6 into local
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	;(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-remote-total < 1)
=>
	(up-full-reset-search)
	;(up-chat-data-to-player my-player-number "Before gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "Before gl-remote-total = %d " g: gl-remote-total)
	(up-set-group search-remote c: 1)
	(up-set-group search-local c: 6)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "After gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "After gl-remote-total = %d " g: gl-remote-total)
	(up-modify-goal gl-temp-one c:= 0)
	;(chat-to-all "Search")
)

;Moving local group into remote group
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-one g:< gl-local-total)
	(up-set-target-object search-local g: gl-temp-one)
=>
	(up-get-object-data object-data-id gl-temp-two)
	(up-add-object-by-id search-remote g: gl-temp-two)
	;(chat-to-all "First jump")
	(up-modify-goal gl-temp-one c:+ 1)
	(up-jump-rule -1)
)

;Clearing group 6
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-one g:== gl-local-total)
	;(up-set-target-object search-remote g: gl-temp-one)
=>
	(up-reset-search 1 1 0 0)
	(up-get-search-state gl-local-total)
	(up-modify-group-flag 0 c: 6)
	(up-reset-group c: 6)
	;(up-create-group 0 0 c: 6)
	;(up-modify-group-flag 1 c: 6)
	(up-modify-goal gl-temp-one c:= 0)
	;(chat-to-all "Recreate group 6")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "gl-remote-total = %d " g: gl-remote-total)
)

;Moving remote group units to local group
(defrule
	(up-group-size c: 6 c:== 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-local-total == 0)
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-one g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-one)
=>
	(up-get-object-data object-data-id gl-temp-two)
	(up-add-object-by-id search-local g: gl-temp-two)
	;(chat-to-all "Second jump")
	(up-modify-goal gl-temp-one c:+ 1)
	(up-jump-rule -1)
)

;remove object from local/remote list before moving it to the other list?

(defrule
	(up-group-size c: 6 c:== 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-local-total == 0)
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-one g:== gl-remote-total)
	;(up-set-target-object search-local g: gl-temp-one)
=>
	;(up-reset-search 0 0 1 1)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "gl-remote-total = %d " g: gl-remote-total)
	;(chat-to-all "Check")
)

(defrule
	(up-group-size c: 6 c:== 0)
	(building-type-count mill > 0)
	(current-age > dark-age)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-one g:== gl-remote-total)
=>
	(up-modify-group-flag 0 c: 1)
	(up-reset-group c: 1)
	(up-create-group 0 0 c: 1)
	(up-modify-group-flag 1 c: 1)
	;(chat-to-all "Recreate group one")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
	;(up-chat-data-to-player my-player-number "gl-remote-total = %d " g: gl-remote-total)
)

;endregion

;region Berries - retasking group 6 villager to build a house
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count house == 2)
	(building-type-count-total house == 3)
=>
	(up-full-reset-search)
	(up-filter-status c: status-pending c: list-active)
	(up-find-status-local c: house c: 1)
	;(chat-to-all "House one")
)

(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count house == 2)
	(building-type-count-total house == 3)
	(up-set-target-object search-local c: 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 6)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "House two")
)

(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count house == 2)
	(building-type-count-total house == 3)
	(up-compare-goal gl-local-total > 0)
=>
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "House three")
)

;endregion

;region Berries - retasking group 6 villager to build a mill
(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill == 0)
	(building-type-count-total mill == 1)
=>
	(up-full-reset-search)
	(up-filter-status c: status-pending c: list-active)
	(up-find-status-local c: mill c: 1)
	;(chat-to-all "Mill one")
)

(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill == 0)
	(building-type-count-total mill == 1)
	(up-set-target-object search-local c: 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 6)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Mill two")
)

(defrule
	(up-group-size c: 6 c:> 0)
	(building-type-count mill == 0)
	(building-type-count-total mill == 1)
	(up-compare-goal gl-local-total > 0)
=>
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Mill three")
)

;endregion

;endregion

;region Wood gathering DUC

	;region Keeping lumber camp group at their tasks - building lumber camp and chopping trees near lumber camp
(defrule
	(current-age == dark-age) ; Starting check if 2nd group exist
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 2)
	;(chat-to-all "One")
)

(defrule
	(current-age == dark-age) ; Checking if lumberjacks are chopping wood while lumber camp is ready
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(building-type-count lumber-camp > 0)
	(up-set-target-object search-local c: 0)
=>
	(up-remove-objects search-local object-data-type == lumberjack-male)
	(up-remove-objects search-local object-data-type == lumberjack-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Two")
)

(defrule
	(current-age == dark-age) ; Checking if lumberjacks are chopping wood or building lumber camp
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(building-type-count-total lumber-camp == 1)
	(building-type-count lumber-camp < 1)
	(up-set-target-object search-local c: 0)
=>
	(up-remove-objects search-local object-data-type == lumberjack-male)
	(up-remove-objects search-local object-data-type == lumberjack-female)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Three")
)

(defrule
	(current-age == dark-age) ; Getting lumber camp's x/y if villager has changed his task
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-local c: 0)
=>
	(set-strategic-number sn-focus-player-number my-player-number)
	(up-filter-status c: status-pending c: list-active)
	(up-find-status-remote c: lumber-camp c: 1)
	(up-filter-status c: status-ready c: list-active)
	(up-find-status-remote c: lumber-camp c: 1)
	;(chat-to-all "Four")
)

(defrule
	(current-age == dark-age) ; Getting villager's x/y if villager has changed his task
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	;(up-get-object-data object-data-target-id gl-temp-one)
	(up-get-object-data object-data-point-x gl-temp-two)
	(up-get-object-data object-data-point-y gl-temp-three)
	;(chat-to-all "Five")
)

(defrule
	(current-age == dark-age) ; Searching for trees, chopped down and the other type after, removing all but first tree
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(up-compare-goal gl-local-total > 0)
	;(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-three > 0)
=>
	(up-set-target-point gl-temp-two)
	(up-reset-search 0 0 1 1)
	(up-filter-distance c: -1 c: 8)
	(up-filter-status c: status-resource c: list-active)
	(up-find-resource c: tree-class c: 2)
	(up-reset-filters)
	(up-filter-distance c: -1 c: 8)
	(up-filter-status c: status-ready c: list-active)
	(up-find-resource c: tree-class c: 20)
	(up-clean-search search-remote object-data-distance search-order-asc)
	(up-remove-objects search-remote object-data-index > 0)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Six")
)

(defrule
	(current-age == dark-age) ; Removing all but first tree, sending villager to chop down that tree
	(up-group-size c: 2 c:>= 1)
	(up-group-size c: 2 c:<= 2)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
	;(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-two > 0)
	(up-compare-goal gl-temp-three > 0)
=>
	(up-target-objects 0 action-default -1 -1)
	;(chat-to-all "Seven")
)
;endregion

	;region Setting first straggler tree as a TC gather point

(defrule
	(unit-type-count villager == 12)
	(timer-triggered twenty-five-seconds-timer)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "One")
)

(defrule
	(unit-type-count villager == 12)
	(timer-triggered twenty-five-seconds-timer)
	(up-set-target-object search-local c: 0)
=>
	(set-strategic-number sn-focus-player-number 0)
	(up-modify-goal gl-temp-one g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-one c:- 3)
	(up-modify-goal gl-temp-two g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-two c:+ 3)
	(up-set-target-point gl-temp-one)
	(up-filter-distance c: -1 c: 7)
	(up-find-resource c: tree-class c: 5)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(chat-to-all "Two")
)

(defrule
	(unit-type-count villager == 12)
	(timer-triggered twenty-five-seconds-timer)
	(up-set-target-object search-local c: 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-target-objects 1 action-gather -1 -1)
	;(chat-to-all "Three")
)

(defrule
	(unit-type-count villager == 13)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "Four")
)

(defrule
	(unit-type-count villager == 13)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-goal gl-temp-one c:= -1)
	(up-modify-goal gl-temp-two c:= -1)
	(up-target-point gl-temp-one action-gather -1 -1)
	;(chat-to-all "Five")
	(disable-self)
)
	;endregion

	;region Setting second straggler tree as a TC gather point

(defrule
	(unit-type-count villager == 17)
	(timer-triggered twenty-five-seconds-timer)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "One")
)

(defrule
	(unit-type-count villager == 17)
	(timer-triggered twenty-five-seconds-timer)
	(up-set-target-object search-local c: 0)
=>
	(set-strategic-number sn-focus-player-number 0)
	(up-modify-goal gl-temp-one g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-one c:- 3)
	(up-modify-goal gl-temp-two g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-two c:+ 3)
	(up-set-target-point gl-temp-one)
	(up-filter-distance c: -1 c: 7)
	(up-find-resource c: tree-class c: 5)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(chat-to-all "Two")
)

(defrule
	(unit-type-count villager == 17)
	(timer-triggered twenty-five-seconds-timer)
	(up-set-target-object search-local c: 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-target-objects 1 action-gather -1 -1)
	;(chat-to-all "Three")
)

(defrule
	(unit-type-count villager == 19)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	;(chat-to-all "Four")
)

(defrule
	(unit-type-count villager == 19)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-goal gl-temp-one c:= -1)
	(up-modify-goal gl-temp-two c:= -1)
	(up-target-point gl-temp-one action-gather -1 -1)
	;(chat-to-all "Five")
	(disable-self)
)
	;endregion

	;region Chopping straggler trees

(defrule
	(current-age == dark-age) ; Starting check if 5th group exist
	(up-group-size c: 5 c:>= 1)
	(up-group-size c: 5 c:<= 5)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 5)
	;(chat-to-all "One")
)

(defrule
	(current-age == dark-age) ; Checking if lumberjacks are chopping wood while lumber camp is ready
	(up-group-size c: 5 c:>= 1)
	(up-group-size c: 5 c:<= 5)
	;(building-type-count lumber-camp > 0)
	(up-set-target-object search-local c: 0)
=>
	(up-remove-objects search-local object-data-type == lumberjack-male)
	(up-remove-objects search-local object-data-type == lumberjack-female)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Two")
)

(defrule
	(current-age == dark-age) ; Searching for trees, chopped down and the other type after, removing all but first tree
	(up-group-size c: 5 c:>= 1)
	(up-group-size c: 5 c:<= 5)
	(up-compare-goal gl-local-total > 0)
=>
	; (up-modify-goal gl-temp-one g:= gl-starting-tc-x)
	; (up-modify-goal gl-temp-one c:- 3)
	; (up-modify-goal gl-temp-two g:= gl-starting-tc-y)
	; (up-modify-goal gl-temp-two c:+ 3)
	(up-set-target-object search-local c: 0)
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(up-set-target-point gl-temp-one)
	;(up-set-target-point gl-starting-tc-x)
	;(up-reset-search 0 0 1 1)
	(set-strategic-number sn-focus-player-number 0)
	(up-filter-distance c: -1 c: 8)
	(up-filter-status c: status-resource c: list-active)
	(up-find-resource c: tree-class c: 2)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "First search = %d " g: gl-remote-total)
	;(up-reset-filters)
	;(up-filter-distance c: -1 c: 8)
	(up-filter-status c: status-ready c: list-active)
	(up-find-resource c: tree-class c: 20)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Second search = %d " g: gl-remote-total)
	(up-clean-search search-remote object-data-distance search-order-asc)
	(up-remove-objects search-remote object-data-index > 0)
	(up-get-search-state gl-local-total)
	;(up-chat-data-to-player my-player-number "Third search = %d " g: gl-remote-total)
	;(chat-to-all "Three")
)

(defrule
	(current-age == dark-age) ; Removing all but first tree, sending villager to chop down that tree
	(up-group-size c: 5 c:>= 1)
	(up-group-size c: 5 c:<= 5)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
	;(up-compare-goal gl-temp-one > 0)
	;(up-compare-goal gl-temp-two > 0)
	;(up-compare-goal gl-temp-three > 0)
=>
	;(up-set-target-object search-remote c: 0)
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Four")
)

	;endregion
	
;endregion

;region Dropping food so next villager can be queued, 5s before it's time to train next villager, Dark Age only
(defrule
	(game-time >= 20)
=>
	(enable-timer twenty-five-seconds-timer 25)
	;(chat-to-all "Timer start")
	(disable-self)
)

(defrule
	(unit-type-count villager c:< 18)
	(timer-triggered twenty-five-seconds-timer)
=>
	(up-full-reset-search)
	(up-find-local c: town-center c: 1)
	(up-get-search-state gl-local-total)
)

(defrule
	(unit-type-count villager < 18)
	(timer-triggered twenty-five-seconds-timer)
	(up-compare-goal gl-local-total c:> 0)
=>
	(up-set-target-object search-local c: 0)
	(up-get-object-data object-data-train-count gl-temp-one)
)

(defrule
	(unit-type-count villager c:< 18)
	(up-compare-goal gl-temp-one c:< 2)
	(timer-triggered twenty-five-seconds-timer)
=>
	;(chat-to-all "Timer triggered")
	(up-drop-resources food c: 1)
	;(enable-timer twenty-five-seconds-timer 25)
)

(defrule
	(unit-type-count villager c:< 18)
	(timer-triggered twenty-five-seconds-timer)
=>
	(enable-timer twenty-five-seconds-timer 25)
)
;endregion

;region Town size changes
(defrule
	(current-age == feudal-age)
=>
	(up-modify-sn sn-maximum-town-size c:+ 5)
	(disable-self)
)

(defrule
	(current-age == castle-age)
=>
	(up-modify-sn sn-maximum-town-size c:+ 15)
	(disable-self)
)

(defrule
	(current-age == imperial-age)
=>
	(up-modify-sn sn-maximum-town-size c:+ 35)
	(disable-self)
)
;endregion

;<<< Trading resources >>>

;region Selling wood
(defrule
	(up-research-status c: castle-age < research-pending)
	(can-sell-commodity wood)
	(gold-amount < 400)
	(wood-amount >= 500)
=>
	(sell-commodity wood)
)

(defrule
	(wood-amount >= 1000)
	(gold-amount <= 500)
=>
	(sell-commodity wood)
)
;endregion

;region Selling food
(defrule
	(can-sell-commodity food)
	(food-amount > 1200)
	(gold-amount <= 500)
=>
	(sell-commodity food)
)
;endregion

;region Selling stone
(defrule
	(can-sell-commodity stone)
=>
	(sell-commodity stone)
)
;endregion

;region Buying food
(defrule
	(up-research-status c: castle-age < research-pending)
	(can-buy-commodity food)
	(food-amount < 1000)
	(gold-amount >= 350)
=>
	(buy-commodity food)
)

(defrule
	(gold-amount >= 1000)
	(food-amount <= 500)
=>
	(buy-commodity food)
)
;endregion

;region Buying wood
(defrule
	(gold-amount >= 1000)
	(wood-amount <= 500)
=>
	(buy-commodity wood)
)
;endregion

(load "Sun Tzu 0.2a\Research")

(load "Sun Tzu 0.2a\Units")

;region Deer - thanks Fireball (: - to be improved - deer from behind a forest
;Stop lure and up-jumps
(defrule
	(game-time > 240)
	(up-compare-goal gl-dlure != 100)
	(up-compare-goal gl-third-deer-id > -1)
=>
	(up-full-reset-search)
	(up-add-object-by-id search-remote g: gl-first-deer-id)
	(up-add-object-by-id search-remote g: gl-second-deer-id)
	(up-add-object-by-id search-remote g: gl-third-deer-id)
	(up-remove-objects search-remote object-data-hitpoints < 1)
	(up-remove-objects search-remote object-data-hitpoints < 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Rule one")
)

(defrule
	(game-time > 240)
	(up-compare-goal gl-dlure != 100)
	(up-compare-goal gl-third-deer-id > -1)
	(up-compare-goal gl-remote-total == 0)
=>
	(set-goal gl-dlure 100)
	;(chat-to-all "Rule two")
)
	
;>>>>>>>>>>> PUSHING THE DEER
(defrule	;doesn't jump shooting the deer, just finding deer, so the vills can still shoot them in unusual cases
	(or (goal gl-dlure 100)
		(game-time < TimeBeforeDeerLuring))	;allow for exploration first
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
=>
	;(chat-to-all "Rule 2")
	(up-full-reset-search)
	(up-jump-rule 12)	;jump over the rules for pushing deer if it is not time
)

(defrule	;search for deer around the town center and pick the closest one
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(or	(up-compare-goal gl-first-deer-id > -1)
		(or	(up-compare-goal gl-second-deer-id > -1)
			(up-compare-goal gl-third-deer-id > -1)))
=>
	(up-full-reset-search)
	(up-set-target-point gl-starting-tc-x)
	;(up-filter-distance c: -1 c: 30); was 50
	;(set-strategic-number sn-focus-player-number 0)
	;(up-find-remote c: prey-animal-class c: 40)
	(up-add-object-by-id search-remote g: gl-first-deer-id)
	(up-add-object-by-id search-remote g: gl-second-deer-id)
	(up-add-object-by-id search-remote g: gl-third-deer-id)
	(up-remove-objects search-remote object-data-hitpoints < 1)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(up-remove-objects search-remote -1 > 0)	;only closest
	(up-get-search-state gl-local-total)	;check how many deer were found
	(up-modify-goal gl-temp-three c:= 0)
	;(chat-to-all "Rule 3")
)

(defrule
	(game-time < 510)
	(or	(up-compare-goal gl-first-deer-id > -1)
		(or	(up-compare-goal gl-second-deer-id > -1)
			(up-compare-goal gl-third-deer-id > -1)))
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-three)
=>
	(up-get-object-data object-data-point-x gl-temp-four)
	(up-get-object-data object-data-point-y gl-temp-five)
	;(up-chat-data-to-player my-player-number "gl-temp-four = %d " g: gl-temp-four)
	;(up-chat-data-to-player my-player-number "gl-temp-five = %d " g: gl-temp-five)
	(up-set-target-point gl-temp-four)
)

(defrule
	(game-time < 510)
	(or	(up-compare-goal gl-first-deer-id > -1)
		(or	(up-compare-goal gl-second-deer-id > -1)
			(up-compare-goal gl-third-deer-id > -1)))
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-set-target-object search-remote g: gl-temp-three)
	(up-point-explored gl-temp-four == explored-no)
=>
	(up-modify-goal gl-temp-three c:+ 1)
	;(chat-to-all "Jump")
	(up-jump-rule -2)
)

(defrule	;get the closest deer's ID into a goal if one was found
	(goal gl-dlure -1)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-remote-total >= 1)	;deer was found
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
=>
	(up-get-object-data object-data-id deer-id)
	(set-goal gl-dlure 0)	;start pushing the deer
	;(chat-to-all "Rule 4")
)

(defrule 	;update chosen deer's position and hp every pass
	(up-set-target-by-id g: deer-id)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
=>
	(up-full-reset-search)
	(up-get-object-data object-data-hitpoints goal)
	;(up-get-point position-object object-x)
	(up-get-object-data object-data-precise-x object-x)
	(up-get-object-data object-data-precise-y object-y)
	;(up-chat-data-to-player my-player-number "object-x = %d " g: object-x)
	;(up-chat-data-to-player my-player-number "object-y = %d " g: object-y)
	;(chat-to-all "Rule 5")
)

;=============================================================

(defrule
	;(up-set-target-by-id g: deer-id)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-compare-goal object-x c:> 0)
=>
	(up-get-point-distance object-x gl-tc-bottom-corner-x gl-distance-bottom-TC-corner)
	(up-get-point-distance object-x gl-tc-upper-corner-x gl-distance-upper-TC-corner)
	(up-get-point-distance object-x gl-tc-left-corner-x gl-distance-left-TC-corner)
	(up-get-point-distance object-x gl-tc-right-corner-x gl-distance-right-TC-corner)
	;(chat-to-all "Rule 6")
	;(up-chat-data-to-player my-player-number "gl-distance-bottom-TC-corner = %d " g: gl-distance-bottom-TC-corner)
	;(up-chat-data-to-player my-player-number "gl-distance-upper-TC-corner = %d " g: gl-distance-upper-TC-corner)
	;(up-chat-data-to-player my-player-number "gl-distance-left-TC-corner = %d " g: gl-distance-left-TC-corner)
	;(up-chat-data-to-player my-player-number "gl-distance-right-TC-corner = %d " g: gl-distance-right-TC-corner)
	(disable-self)
)

(defrule
	(up-compare-goal object-x c:> 0)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-deer-found c:< 0)
	(up-compare-goal gl-distance-left-TC-corner g:<= gl-distance-right-TC-corner)
	(up-compare-goal gl-distance-bottom-TC-corner g:<= gl-distance-left-TC-corner)
=>
	;(chat-to-all "Zone 1") ; Lower West
	(up-modify-goal gl-deer-found c:= 1)
	(up-modify-goal gl-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-deer-destination-x c:- 1)
	(up-modify-goal gl-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-deer-destination-x c:- 1)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-deer-destination-y c:- 1)
	(up-modify-goal gl-temp-deer-destination-x c:* 100)
	(up-modify-goal gl-temp-deer-destination-y c:* 100)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	; edit deer destination
	(disable-self)
)

(defrule
	;(up-set-target-by-id g: deer-id)
	(up-compare-goal object-x c:> 0)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-deer-found c:< 0)
	(up-compare-goal gl-distance-right-TC-corner g:<= gl-distance-left-TC-corner)
	(up-compare-goal gl-distance-bottom-TC-corner g:<= gl-distance-right-TC-corner)
=>
	;(chat-to-all "Zone 2") ; Lower East
	(up-modify-goal gl-deer-found c:= 1)
	(up-modify-goal gl-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-deer-destination-x c:- 1)
	(up-modify-goal gl-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-deer-destination-x c:+ 1)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x c:* 100)
	(up-modify-goal gl-temp-deer-destination-y c:* 100)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	(disable-self)
)

(defrule
	;(up-set-target-by-id g: deer-id)
	(up-compare-goal object-x c:> 0)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-deer-found c:< 0)
	(up-compare-goal gl-distance-upper-TC-corner g:<= gl-distance-left-TC-corner)
	(up-compare-goal gl-distance-upper-TC-corner g:<= gl-distance-right-TC-corner)
=>
	;(chat-to-all "Zone 3") ; Upper
	(up-modify-goal gl-deer-found c:= 1)
	(up-modify-goal gl-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-deer-destination-x c:+ 1)
	(up-modify-goal gl-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-deer-destination-x c:- 1)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-deer-destination-y c:- 1)
	(up-modify-goal gl-temp-deer-destination-x c:* 100)
	(up-modify-goal gl-temp-deer-destination-y c:* 100)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	(disable-self)
)

(defrule
	;(up-set-target-by-id g: deer-id)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-compare-goal object-x c:> 0)
	(up-compare-goal gl-deer-found c:< 0)
	(up-compare-goal gl-distance-left-TC-corner g:<= gl-distance-bottom-TC-corner)
	(up-compare-goal gl-distance-left-TC-corner g:<= gl-distance-upper-TC-corner)
=>
	;(chat-to-all "Zone 4") ; Left
	(up-modify-goal gl-deer-found c:= 1)
	(up-modify-goal gl-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-deer-destination-x c:- 1)
	(up-modify-goal gl-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-deer-destination-y c:- 1)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-deer-destination-x c:- 1)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x c:* 100)
	(up-modify-goal gl-temp-deer-destination-y c:* 100)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	(disable-self)
)

(defrule
	;(up-set-target-by-id g: deer-id)
	(up-compare-goal object-x c:> 0)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-deer-found c:< 0)
	(up-compare-goal gl-distance-right-TC-corner g:<= gl-distance-bottom-TC-corner)
	(up-compare-goal gl-distance-right-TC-corner g:<= gl-distance-upper-TC-corner)
=>
	;(chat-to-all "Zone 5") ; Right
	(up-modify-goal gl-deer-found c:= 1)
	(up-modify-goal gl-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-deer-destination-x c:+ 1)
	(up-modify-goal gl-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-deer-destination-x c:- 1)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-deer-destination-y c:+ 1)
	(up-modify-goal gl-temp-deer-destination-x c:* 100)
	(up-modify-goal gl-temp-deer-destination-y c:* 100)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	(disable-self)
)
;=============================================================

(defrule
	(up-compare-goal goal > 0)	;if the deer is still alive
	(up-set-target-by-id g: deer-id)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
=>
	(up-copy-point point-x object-x)	;copy deer position into a second point
	(up-lerp-tiles point-x gl-deer-destination-x c: -75)	;move point-x one tile away from tc so the scout will be behind the deer
	;(up-bound-point point-x point-x)	;make sure point isn't off the map
	(up-set-target-point point-x)
	(set-strategic-number sn-target-point-adjustment 6)	;helps the scout positioning closer to the deer
	(up-full-reset-search)
	(up-find-local c: scout-unit c: 1)
	(up-target-point point-x action-move -1 stance-no-attack)
	;(up-chat-data-to-player my-player-number "gl-temp-deer-destination-x = %d " g: gl-temp-deer-destination-x)
	;(up-chat-data-to-player my-player-number "gl-temp-deer-destination-y = %d " g: gl-temp-deer-destination-y)
	;(up-chat-data-to-player my-player-number "object-x = %d " g: object-x)
	;(up-chat-data-to-player my-player-number "object-y = %d " g: object-y)
	;(up-chat-data-to-player my-player-number "point-x = %d " g: point-x)
	;(up-chat-data-to-player my-player-number "point-y = %d " g: point-y)
	(set-strategic-number sn-target-point-adjustment 0)
	;(chat-to-all "Rule 6")
)

;>>>>>>>>>>> SHOOTING THE DEER
(defrule
	(up-compare-goal goal > 0)	;if the deer is still alive
	(up-set-target-by-id g: deer-id)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	;(true)
=>
	(up-full-reset-search)
	;(set-strategic-number sn-target-point-adjustment 6)
	;(set-strategic-number sn-target-point-adjustment 6)
	;(up-chat-data-to-player my-player-number "gl-deer-destination-x = %d " g: gl-deer-destination-x)
	;(up-chat-data-to-player my-player-number "gl-deer-destination-y = %d " g: gl-deer-destination-y)
	(up-modify-goal gl-deer-destination-x c:/ 100)
	(up-modify-goal gl-deer-destination-y c:/ 100)
	;(up-chat-data-to-player my-player-number "gl-deer-destination-x = %d " g: gl-deer-destination-x)
	;(up-chat-data-to-player my-player-number "gl-deer-destination-y = %d " g: gl-deer-destination-y)
	(up-set-target-point gl-deer-destination-x)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	;(set-strategic-number sn-target-point-adjustment 0)
	;<<< Removed >>>
	; (up-filter-distance c: -1 c: DistanceToShootDeer)
	; (set-strategic-number sn-focus-player-number 0)
	; (up-find-remote c: prey-animal-class c: 5)
	;<<< Removed >>>
	;<<< Added >>>
	(up-add-object-by-id search-remote g: deer-id)
	;(up-add-object-by-id search-remote gl-first-deer-id)
	;(up-add-object-by-id search-remote gl-second-deer-id)
	;(up-add-object-by-id search-remote gl-third-deer-id)
	;(up-modify-goal gl-temp-five c:= 0)
	(up-modify-goal gl-temp-five g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-five c:- 3)
	(up-modify-goal gl-temp-six g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-six c:- 3)
	(up-modify-goal gl-temp-seven g:= gl-starting-tc-x)
	(up-modify-goal gl-temp-seven c:+ 2)
	(up-modify-goal gl-temp-eight g:= gl-starting-tc-y)
	(up-modify-goal gl-temp-eight c:+ 2)
	;<<< Added >>>
	(up-remove-objects search-remote object-data-hitpoints < 1)	;only live deer
	(up-get-search-state gl-local-total)	;see how many were found
	;(set-strategic-number sn-target-point-adjustment 0)
	;(up-chat-data-to-player my-player-number "gl-remote-total = %d " g: gl-remote-total)
	;(chat-to-all "Rule 7")
)

;Distance check
(defrule
	(up-compare-goal goal > 0)	;if the deer is still alive
	(up-set-target-by-id g: deer-id)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-remote-total > 0)
	;(true)
=>
	(up-get-object-data object-data-point-x gl-temp-nine)
	(up-get-object-data object-data-point-y gl-temp-ten)
	;(chat-to-all "Test")
)

(defrule	;shoot the deer if not hunting boar and one is near the tc
	(up-compare-goal gl-remote-total >= 1)	;deer found near tc
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-timer-status t-misc != timer-running)	;this is so the command doesnt loop continuously
	;(or	(dropsite-min-distance live-boar == -1)
	;	(dropsite-min-distance live-boar >= 10))
	(up-compare-goal gl-remote-total > 0)
	(up-compare-goal gl-temp-nine g:>= gl-temp-five)
	(up-compare-goal gl-temp-nine g:<= gl-temp-seven)
	(up-compare-goal gl-temp-ten g:>= gl-temp-six)
	(up-compare-goal gl-temp-ten g:<= gl-temp-eight)
=>
	;(up-full-reset-search)
	;(set-strategic-number sn-target-point-adjustment 6)
	(up-modify-goal gl-deer-destination-x c:/ 100)
	(up-modify-goal gl-deer-destination-y c:/ 100)
	(up-set-target-point gl-deer-destination-x)
	(up-modify-goal gl-deer-destination-x c:* 100)
	(up-modify-goal gl-deer-destination-y c:* 100)
	;(set-strategic-number sn-target-point-adjustment 0)
	; (up-filter-distance c: -1 c: 8)
	; (up-find-local c: shepherd-male c: 10)
	; (up-find-local c: shepherd-female c: 10)
	; (up-find-local c: hunter-male c: 10)
	; (up-find-local c: hunter-female c: 10)
	(up-find-local c: town-center c: 1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "TC rule")
)

(defrule	;shoot the deer if not hunting boar and one is near the tc
	(up-compare-goal gl-remote-total >= 1)	;deer found near tc
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-timer-status t-misc != timer-running)	;this is so the command doesnt loop continuously
	;(or	(dropsite-min-distance live-boar == -1)
	;	(dropsite-min-distance live-boar >= 10))
	;(up-set-target-object search-local c: 0)
	(up-compare-goal gl-local-total == 1)
	(up-compare-goal gl-temp-nine g:>= gl-temp-five)
	(up-compare-goal gl-temp-nine g:<= gl-temp-seven)
	(up-compare-goal gl-temp-ten g:>= gl-temp-six)
	(up-compare-goal gl-temp-ten g:<= gl-temp-eight)
=>
	(up-set-target-object search-remote c: 0)
	(up-target-objects 1 action-gather -1 -1)
	(up-set-target-object search-local c: 0)
	(up-reset-search 1 1 0 0)
	(up-reset-filters)
	(up-set-group search-local c: 1)
	(up-remove-objects search-local object-data-type == builder-male)
	(up-remove-objects search-local object-data-type == builder-female)
	(up-remove-objects search-local object-data-group-flag == 4)
	(up-set-precise-target-point gl-deer-destination-x)
	(up-clean-search search-local object-data-precise-distance search-order-asc)
	(up-remove-objects search-local object-data-index > 4)
	; (up-clean-search search-local object-data-distance search-order-asc)
	; (up-remove-objects search-local object-data-action == actionid-build)
	; (up-remove-objects search-local object-data-target == tree-class)
	; (up-remove-objects search-local object-data-target-id g:== gl-boar-id)
	; (up-remove-objects search-local -1 > VillsToShootDeer)	;number of vills to shoot deer
	(up-remove-objects search-remote -1 > 0)	;closest deer
	;(up-modify-sn sn-enable-boar-hunting c: 2)
	;(up-target-objects 0 action-default -1 -1)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Deer rule")
)

(defrule	;shoot the deer if not hunting boar and one is near the tc
	(up-compare-goal gl-remote-total >= 1)	;deer found near tc
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-timer-status t-misc != timer-running)	;this is so the command doesnt loop continuously
	;(or	(dropsite-min-distance live-boar == -1)
	;	(dropsite-min-distance live-boar >= 10))
	(up-compare-goal gl-local-total > 1)
	(up-compare-goal gl-temp-nine g:>= gl-temp-five)
	(up-compare-goal gl-temp-nine g:<= gl-temp-seven)
	(up-compare-goal gl-temp-ten g:>= gl-temp-six)
	(up-compare-goal gl-temp-ten g:<= gl-temp-eight)
=>
	(up-target-objects 1 action-garrison -1 -1)
	(up-modify-goal gl-deer-shot c:= 1)
	(enable-timer t-misc 5)	;wait awhile before allowing this rule to fire again
	;(chat-to-all "Deer shot")
	;(chat-to-all "Rule 8")
	(up-remove-objects search-remote -1 > -1)	;clear remote list
	;(chat-to-all "Kill rule")
)

(defrule 	;reset the system once deer is shot
	(up-compare-goal goal < 2)	;hp
	;(up-research-status c: feudal-age < research-pending)
	(game-time < 510)
	(up-compare-goal gl-temp-three g:< gl-remote-total)
	(up-compare-goal gl-dlure >= 0)
	(up-compare-goal gl-dlure != 100)	;not done luring yet
	; (up-compare-goal gl-temp-nine g:>= gl-temp-five)
	; (up-compare-goal gl-temp-nine g:<= gl-temp-seven)
	; (up-compare-goal gl-temp-ten g:>= gl-temp-six)
	; (up-compare-goal gl-temp-ten g:<= gl-temp-eight)
=>
	;(set-strategic-number sn-target-point-adjustment 6)
	(up-reset-unit c: scout-unit)
	(set-goal gl-dlure -1)
	(set-goal deer-id -1)
	;(up-reset-scouts)
	;(chat-to-all "Rule 9")
	(up-modify-goal gl-temp-one g:= gl-temp-deer-destination-x)
	(up-modify-goal gl-temp-two g:= gl-temp-deer-destination-y)
	(up-modify-goal gl-temp-deer-destination-x g:= gl-deer-destination-x)
	(up-modify-goal gl-temp-deer-destination-y g:= gl-deer-destination-y)
	(up-modify-goal gl-deer-destination-x g:= gl-temp-one)
	(up-modify-goal gl-deer-destination-y g:= gl-temp-two)
	;(set-strategic-number sn-target-point-adjustment 0)
)

(defrule
	(up-compare-goal gl-dlure == 0)	;deer was found
=>
	;(chat-to-all "Anti scout Jump")
	(up-jump-rule 40);skipping scouting and sheep capture 4 + 13 + 23
)
;endregion

;region Capturing sheep out of reach but close enough to a scout
(defrule
	(up-research-status c: feudal-age < research-pending)
	(up-group-size c: 9 > 0)
	(timer-triggered three-second-timer)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
	;(chat-to-all "Sheep one")
)

(defrule
	(up-research-status c: feudal-age < research-pending)
	(up-group-size c: 9 > 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(up-set-target-point gl-temp-one)
    (set-strategic-number sn-focus-player-number 0)
	(up-filter-distance c: -1 c: 12)
	(up-find-remote c: livestock-class c: 4)
	;(chat-to-all "Sheep two")
)

(defrule
	(up-research-status c: feudal-age < research-pending)
	(up-group-size c: 9 > 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-remote c: 0)
=>
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(chat-to-all "Sheep three")
)

(defrule
	(up-research-status c: feudal-age < research-pending)
	(up-group-size c: 9 > 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-remote c: 0)
=>
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(up-set-target-point gl-temp-one)
	(up-target-point gl-temp-one action-default -1 -1)
	;(chat-to-all "Sheep four")
	(up-jump-rule 36); skipping scouting 13 + 23
	;(enable-timer three-second-timer 3)
)
;endregion

;region Scouting - avoiding forests and map edge - to be improved - replace older system?
;Getting x/y of 4 points of interest - above/below/to the left/right of scout
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
	(up-set-target-point gl-starting-tc-x)
	(up-get-object-data object-data-point-x  gl-temp-one);scout's X coordinate
	(up-get-object-data object-data-point-y  gl-temp-two);scout's Y coordinate
	;Getting x/y of point above scout - away from TC
	(up-modify-goal gl-temp-three g:= gl-temp-one)
	(up-modify-goal gl-temp-four g:= gl-temp-two)
	(up-lerp-tiles gl-temp-three gl-starting-tc-x c: -4)
	;Getting x/y of point opposite to scouting direction
	(up-modify-goal gl-temp-five g:= gl-temp-one)
	(up-modify-goal gl-temp-six g:= gl-temp-two)
	(up-modify-goal gl-temp-eleven g:= gl-scouting-direction)
	(up-modify-goal gl-temp-eleven c:* 4)
	(up-cross-tiles gl-temp-five gl-starting-tc-x g: gl-temp-eleven)
	;Getting x/y of point below scout - towards TC
	(up-modify-goal gl-temp-seven g:= gl-temp-one)
	(up-modify-goal gl-temp-eight g:= gl-temp-two)
	(up-lerp-tiles gl-temp-seven gl-starting-tc-x c: 4)
	;Getting x/y of point towards scouting direction
	(up-modify-goal gl-temp-nine g:= gl-temp-one)
	(up-modify-goal gl-temp-ten g:= gl-temp-two)
	(up-modify-goal gl-temp-eleven g:= gl-scouting-direction)
	(up-modify-goal gl-temp-eleven c:* -4)
	(up-cross-tiles gl-temp-nine gl-starting-tc-x g: gl-temp-eleven)
	(up-get-point position-map-size gl-temp-twelve)
	(up-modify-goal gl-temp-twelve c:= 1);map boundaries
	(up-modify-goal gl-temp-thirteen c:- 1);map boundaries
	;(up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	;(up-chat-data-to-player my-player-number "Away X = %d " g: gl-temp-three)
	;(up-chat-data-to-player my-player-number "Away Y = %d " g: gl-temp-four)
	;(up-chat-data-to-player my-player-number "Opposite X = %d " g: gl-temp-five)
	;(up-chat-data-to-player my-player-number "Opposite Y = %d " g: gl-temp-six)
	;(up-chat-data-to-player my-player-number "Towards TC X = %d " g: gl-temp-seven)
	;(up-chat-data-to-player my-player-number "Towards TC Y = %d " g: gl-temp-eight)
	;(up-chat-data-to-player my-player-number "Towards X = %d " g: gl-temp-nine)
	;(up-chat-data-to-player my-player-number "Towards Y = %d " g: gl-temp-ten)
)

;Resetting values
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
=>
	(up-modify-goal gl-temp-fourteen c:= 0)
	(up-modify-goal gl-temp-fifteen c:= 0)
	(up-modify-goal gl-temp-sixteen c:= 0)
	(up-modify-goal gl-temp-seventeen c:= 0)
)

;Checking first x/y pair
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(up-compare-goal gl-temp-three g:< gl-temp-twelve);X < 1
		(or	(up-compare-goal gl-temp-four g:< gl-temp-twelve);Y < 1
			(or	(up-compare-goal gl-temp-three g:> gl-temp-thirteen);X > map size
				(or	(up-compare-goal gl-temp-four g:> gl-temp-thirteen);Y > map size
					(up-point-contains gl-temp-three c: tree-class)))))
=>
	(up-modify-goal gl-temp-fourteen c:= 1)
	;(chat-to-all "There's a tree or map edge above me")
)

;Checking second x/y pair
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(up-compare-goal gl-temp-five g:< gl-temp-twelve);X < 1
		(or	(up-compare-goal gl-temp-six g:< gl-temp-twelve);Y < 1
			(or	(up-compare-goal gl-temp-five g:> gl-temp-thirteen);X > map size
				(or	(up-compare-goal gl-temp-six g:> gl-temp-thirteen);Y > map size
					(up-point-contains gl-temp-five c: tree-class)))))
=>
	(up-modify-goal gl-temp-fifteen c:= 1)
	;(chat-to-all "There's a tree or map edge behind me")
)

;Checking third x/y pair
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(up-compare-goal gl-temp-seven g:< gl-temp-twelve);X < 1
		(or	(up-compare-goal gl-temp-eight g:< gl-temp-twelve);Y < 1
			(or	(up-compare-goal gl-temp-seven g:> gl-temp-thirteen);X > map size
				(or	(up-compare-goal gl-temp-eight g:> gl-temp-thirteen);Y > map size
					(up-point-contains gl-temp-seven c: tree-class)))))
=>
	(up-modify-goal gl-temp-sixteen c:= 1)
	;(chat-to-all "There's a tree or map edge below me")
)

;Checking second x/y pair
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(up-compare-goal gl-temp-nine g:< gl-temp-twelve);X < 1
		(or	(up-compare-goal gl-temp-ten g:< gl-temp-twelve);Y < 1
			(or	(up-compare-goal gl-temp-nine g:> gl-temp-thirteen);X > map size
				(or	(up-compare-goal gl-temp-ten g:> gl-temp-thirteen);Y > map size
					(up-point-contains gl-temp-nine c: tree-class)))))
=>
	(up-modify-goal gl-temp-seventeen c:= 1)
	;(chat-to-all "There's a tree or map edge in front of me")
)

;Going forward
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	; (or	(up-compare-goal gl-temp-fifteen == 0)
		; (and	(up-compare-goal gl-temp-fifteen == 1)
				; (or	(and	(up-compare-goal gl-temp-fourteen == 0)
							; (up-compare-goal gl-temp-sixteen == 0))
				; (and	(up-compare-goal gl-temp-fourteen == 1)
						; (up-compare-goal gl-temp-sixteen == 1)))))
	(or	(and	(up-compare-goal gl-temp-fourteen == 1)
				(up-compare-goal gl-temp-sixteen == 1))
		(or	(and	(up-compare-goal gl-temp-fourteen == 1)
					(and	(up-compare-goal gl-temp-fifteen == 0)
							(up-compare-goal gl-temp-sixteen == 0)))
			(or	(and	(up-compare-goal gl-temp-fourteen == 0)
						(and	(up-compare-goal gl-temp-fifteen == 0)
								(up-compare-goal gl-temp-sixteen == 1)))
				(and	(up-compare-goal gl-temp-fourteen == 0)
						(and	(up-compare-goal gl-temp-fifteen == 1)
								(up-compare-goal gl-temp-sixteen == 0))))))
	(up-compare-goal gl-temp-seventeen == 0)
=>
	(up-target-point gl-temp-nine action-move -1 -1)
	;(chat-to-all "Going forward")
	(up-jump-rule 29)
)

;Going towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(and	(up-compare-goal gl-temp-fourteen == 1)
				(up-compare-goal gl-temp-fifteen == 1))
		;(or	(and	(up-compare-goal gl-temp-fourteen == 0)
		;			(up-compare-goal gl-temp-fifteen == 0))
			(and	(up-compare-goal gl-temp-fourteen == 0)
					(up-compare-goal gl-temp-fifteen == 1)));)
	(up-compare-goal gl-temp-sixteen == 0)
	(up-compare-goal gl-temp-seventeen == 1)
=>
	(up-target-point gl-temp-seven action-move -1 -1)
	;(chat-to-all "Going towards TC")
	(up-jump-rule 28)
)

;Going forward and towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(or	(and	(up-compare-goal gl-temp-fourteen == 1)
				(and	(up-compare-goal gl-temp-fifteen == 0)
						(and	(up-compare-goal gl-temp-sixteen == 0)
								(up-compare-goal gl-temp-seventeen == 1))))
		(and	(up-compare-goal gl-temp-fourteen == 0)
				(and	(up-compare-goal gl-temp-fifteen == 1)
						(and	(up-compare-goal gl-temp-sixteen == 1)
								(up-compare-goal gl-temp-seventeen == 0)))))
=>
	(up-modify-goal gl-temp-seven g:+ gl-temp-nine)
	(up-modify-goal gl-temp-seven c:/ 2)
	(up-modify-goal gl-temp-eight g:+ gl-temp-ten)
	(up-modify-goal gl-temp-eight c:/ 2)
	(up-lerp-tiles gl-temp-one gl-temp-seven c: 4)
	(up-target-point gl-temp-one action-move -1 -1)
	;(chat-to-all "Going forward and towards TC")
	(up-jump-rule 27)
)

;Going forward and towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	;(or	(and	(up-compare-goal gl-temp-fourteen == 1)
	;			(and	(up-compare-goal gl-temp-fifteen == 1)
	;					(and	(up-compare-goal gl-temp-sixteen == 0)
	;							(up-compare-goal gl-temp-seventeen == 0))))
		(and	(up-compare-goal gl-temp-fourteen == 0)
				(and	(up-compare-goal gl-temp-fifteen == 0)
						(and	(up-compare-goal gl-temp-sixteen == 1)
								(up-compare-goal gl-temp-seventeen == 1))));)
=>
	(up-modify-goal gl-temp-nine g:+ gl-temp-three)
	(up-modify-goal gl-temp-nine c:/ 2)
	(up-modify-goal gl-temp-ten g:+ gl-temp-four)
	(up-modify-goal gl-temp-ten c:/ 2)
	(up-lerp-tiles gl-temp-one gl-temp-nine c: 4)
	(up-target-point gl-temp-one action-move -1 -1)
	;(chat-to-all "Going forward and away from TC")
	(up-jump-rule 26)
)

;Going backwards and towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-temp-fourteen == 1)
	(up-compare-goal gl-temp-fifteen == 0)
	(up-compare-goal gl-temp-sixteen == 1)
	(up-compare-goal gl-temp-seventeen == 1)
=>
	;(up-modify-goal gl-temp-seven g:+ gl-temp-five)
	;(up-modify-goal gl-temp-seven c:/ 2)
	;(up-modify-goal gl-temp-eight g:+ gl-temp-six)
	;(up-modify-goal gl-temp-eight c:/ 2)
	;(up-lerp-tiles gl-temp-one gl-temp-seven c: 3)
	;(up-target-point gl-temp-one action-move -1 -1)
	(up-modify-goal gl-scouting-direction c:* -1)
	;(chat-to-all "Reverse scouting direction")
	(up-lerp-tiles gl-temp-five gl-starting-tc-x c: 2)
	(up-target-point gl-temp-five action-move -1 -1)
	(up-jump-rule 25)
)

;Going to TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-temp-fourteen == 1)
	(up-compare-goal gl-temp-fifteen == 1)
	(up-compare-goal gl-temp-sixteen == 1)
	(up-compare-goal gl-temp-seventeen == 1)
=>
	(up-target-point gl-starting-tc-x action-move -1 -1)
	;(chat-to-all "Going to TC")
	(up-jump-rule 24)
)

;Going away from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-temp-fourteen == 0)
	(up-compare-goal gl-temp-fifteen == 1)
	(up-compare-goal gl-temp-sixteen == 1)
	(up-compare-goal gl-temp-seventeen == 1)
=>
	(up-target-point gl-temp-three action-move -1 -1)
	;(chat-to-all "Going away from TC")
	(up-jump-rule 23)
)
;endregion

;region Scouting - control group 9
; <<< Creating control group 9 >>>
; Looking for a scout if control group 1 is empty
(defrule
	(up-group-size c: 9 < 1)
=>
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	;(chat-to-all "One")
)

; Adding scout to the control group 1
(defrule
	(up-group-size c: 9 < 1)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-group-flag 0 c: 9)
	(up-reset-group c: 9)
	(up-create-group 0 0 c: 9)
	(up-modify-group-flag 1 c: 9)
	;(chat-to-all "Two")
)

; <<< Choosing scouting direction of rotation based on sheep placement>>>
; Selecting control group 9
(defrule
	(up-compare-goal gl-scouting-direction == 0) ;initial value was set to 0
	(up-group-size c: 9 > 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
	;(chat-to-all "Three")
)

; Getting x/y of a scout, then both cross points to check which one is closer to the sheep
(defrule
	(up-compare-goal gl-scouting-direction == 0)
	(up-group-size c: 9 > 0)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(up-modify-goal gl-temp-three g:= gl-temp-one)
	(up-modify-goal gl-temp-four g:= gl-temp-two)
	(up-cross-tiles gl-temp-three gl-starting-tc-x c: 5)
	(up-modify-goal gl-temp-five g:= gl-temp-one)
	(up-modify-goal gl-temp-six g:= gl-temp-two)
	(up-cross-tiles gl-temp-five gl-starting-tc-x c: -5)
	;(chat-to-all "Four")
)

; Looking for sheep
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction == 0)
=>
	;(up-full-reset-search)
	;(up-set-group search-local c: 9)
	(set-strategic-number sn-focus-player-number 0)
	(up-find-remote c: livestock-class c: 8)
	(set-strategic-number sn-focus-player-number my-player-number)
	(up-find-remote c: livestock-class c: 8)
	;(chat-to-all "Five")
)

; Sorting by closest distance to scout
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction == 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
=>
	(up-set-target-point gl-temp-one)
	(up-clean-search search-remote object-data-distance search-order-asc)
	;(chat-to-all "Six")
)

; Getting distance between sheep and both cross-points
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction == 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
=>
	(up-set-target-point gl-temp-three)
	(up-get-object-data object-data-distance gl-temp-seven)
	(up-set-target-point gl-temp-five)
	(up-get-object-data object-data-distance gl-temp-eight)
	;(chat-to-all "Seven")
)

; If left cross-point is closer to the sheep, go left
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction == 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
	(up-compare-goal gl-temp-seven > 0)
	(up-compare-goal gl-temp-eight > 0)
	(up-compare-goal gl-temp-seven g:<= gl-temp-eight)
=>
	(up-modify-goal gl-scouting-direction c:= -1)
	;(chat-to-all "Eight - go left")
)

; If right cross-point is closer to the sheep, go right
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction == 0)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
	(up-compare-goal gl-temp-seven > 0)
	(up-compare-goal gl-temp-eight > 0)
	(up-compare-goal gl-temp-seven g:> gl-temp-eight)
=>
	(up-modify-goal gl-scouting-direction c:= 1)
	;(chat-to-all "Nine - go right")
)

; <<< Scouting >>>
; Selecting control group 9
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-game-stage < 1)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
	;(chat-to-all "Ten")
)

; Getting x/y of a scout
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
=>
	(up-set-target-point gl-starting-tc-x)
	(up-get-object-data object-data-point-x  gl-temp-one)
	;(up-chat-data-to-player my-player-number "X = %d " g: gl-temp-one)
	(up-get-object-data object-data-point-y  gl-temp-two)
	;(up-chat-data-to-player my-player-number "Y = %d " g: gl-temp-two)
	;(up-cross-tiles gl-temp-one gl-starting-tc-x c: 10)
	;(up-bound-precise-point gl-temp-one 0 c: 2)
	;(chat-to-all "Eleven")
	;(up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
)

; Getting lerp away from TC, away cross, in-between points
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
=>
	(up-modify-goal gl-temp-three g:= gl-temp-one)
	(up-modify-goal gl-temp-four g:= gl-temp-two)
	(up-lerp-tiles gl-temp-three gl-starting-tc-x c: -4)
	(up-modify-goal gl-temp-five g:= gl-temp-one)
	(up-modify-goal gl-temp-six g:= gl-temp-two)
	(up-modify-goal gl-temp-thirteen g:= gl-scouting-direction)
	(up-modify-goal gl-temp-thirteen c:* -4)
	(up-cross-tiles gl-temp-five gl-starting-tc-x g: gl-temp-thirteen)
	(up-modify-goal gl-temp-seven g:= gl-temp-three)
	(up-modify-goal gl-temp-seven g:+ gl-temp-five)
	(up-modify-goal gl-temp-seven c:/ 2)
	(up-modify-goal gl-temp-eight g:= gl-temp-four)
	(up-modify-goal gl-temp-eight g:+ gl-temp-six)
	(up-modify-goal gl-temp-eight c:/ 2)
	(up-modify-goal gl-temp-nineteen g:= gl-temp-seven)
	(up-modify-goal gl-temp-twenty g:= gl-temp-eight)
	(up-modify-goal gl-temp-seven g:= gl-temp-one)
	(up-modify-goal gl-temp-eight g:= gl-temp-two)
	(up-lerp-tiles gl-temp-seven gl-temp-nineteen c: 4)
	;(chat-to-all "Twelve")
)

; Getting lerp towards TC, towards cross
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
=>	
	(up-modify-goal gl-temp-nine g:= gl-temp-one)
	(up-modify-goal gl-temp-ten g:= gl-temp-two)
	;(up-chat-data-to-player my-player-number "Pre X = %d " g: gl-temp-nine)
	;(up-chat-data-to-player my-player-number "Pre y = %d " g: gl-temp-ten)
	;(up-lerp-tiles gl-temp-nine gl-starting-tc-x c: 4)
	(up-lerp-tiles gl-temp-nine gl-starting-tc-x c: 4);was -three
	;(up-chat-data-to-player my-player-number "Post X = %d " g: gl-temp-nine)
	;(up-chat-data-to-player my-player-number "Post y = %d " g: gl-temp-ten)
	(up-modify-goal gl-temp-eleven g:= gl-temp-nine)
	(up-modify-goal gl-temp-eleven g:+ gl-temp-five)
	(up-modify-goal gl-temp-eleven c:/ 2)
	(up-modify-goal gl-temp-twelve g:= gl-temp-ten)
	(up-modify-goal gl-temp-twelve g:+ gl-temp-six)
	(up-modify-goal gl-temp-twelve c:/ 2)
	(up-modify-goal gl-temp-nineteen g:= gl-temp-eleven)
	(up-modify-goal gl-temp-twenty g:= gl-temp-twelve)
	(up-modify-goal gl-temp-eleven g:= gl-temp-one)
	(up-modify-goal gl-temp-twelve g:= gl-temp-two)
	(up-lerp-tiles gl-temp-eleven gl-temp-nineteen c: 4)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Lerp X = %d " g: gl-temp-three)
	; (up-chat-data-to-player my-player-number "Lerp Y = %d " g: gl-temp-four)
	; (up-chat-data-to-player my-player-number "Cross X = %d " g: gl-temp-five)
	; (up-chat-data-to-player my-player-number "Cross Y = %d " g: gl-temp-six)
	; (up-chat-data-to-player my-player-number "Cross X = %d " g: gl-temp-seven)
	; (up-chat-data-to-player my-player-number "Cross Y = %d " g: gl-temp-eight)
	; (up-set-target-point gl-temp-three)
	(up-modify-goal gl-temp-fourteen c:= 4)
	(up-modify-goal gl-temp-fifteen c:= 4)
	(up-modify-goal gl-temp-sixteen c:= 2)
	(up-modify-goal gl-temp-seventeen c:= 4)
	(up-modify-goal gl-temp-eighteen c:= 2)
	;(chat-to-all "Thirteen")
)

; Getting map size
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
=>
	(up-get-point position-map-size gl-temp-nineteen)
	(up-modify-goal gl-temp-nineteen c:= 1)
	(up-modify-goal gl-temp-twenty c:- 1)
	;(up-chat-data-to-player my-player-number "Map X = %d " g: gl-temp-nineteen)
	;(up-chat-data-to-player my-player-number "Map Y = %d " g: gl-temp-twenty)
)

; Getting closest unexplored lerp point away from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-twelve > 0)
	(up-point-explored gl-temp-three != explored-no)
	(up-compare-goal gl-temp-three g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-four g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-three g:< gl-temp-twenty)
	(up-compare-goal gl-temp-four g:< gl-temp-twenty)
	;(up-compare-goal gl-temp-ten < 7)
=>
	;(up-set-target-point gl-temp-three)
	(up-lerp-tiles gl-temp-three gl-temp-one c: -1)
	(up-modify-goal gl-temp-fourteen c:+ 1)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Lerp X = %d " g: gl-temp-three)
	; (up-chat-data-to-player my-player-number "Lerp Y = %d " g: gl-temp-four)
	; (up-chat-data-to-player my-player-number "Distance = %d " g: gl-temp-ten)
	;(chat-to-all "Fourteen")
	(up-jump-rule -1)
)

; Getting closest unexplored cross point away from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-twelve > 0)
	(up-point-explored gl-temp-five != explored-no)
	(up-compare-goal gl-temp-five g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-six g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-five g:< gl-temp-twenty)
	(up-compare-goal gl-temp-six g:< gl-temp-twenty)
	;(up-compare-goal gl-temp-eleven < 7)
=>
	;(up-set-target-point gl-temp-five)
	(up-lerp-tiles gl-temp-five gl-temp-one c: -1)
	(up-modify-goal gl-temp-fifteen c:+ 1)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Cross X = %d " g: gl-temp-five)
	; (up-chat-data-to-player my-player-number "Cross Y = %d " g: gl-temp-six)
	; (up-chat-data-to-player my-player-number "Distance = %d " g: gl-temp-eleven)
	;(chat-to-all "Fifteen")
	(up-jump-rule -1)
)

; Getting closest unexplored in-between point away from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-twelve > 0)
	(up-point-explored gl-temp-seven != explored-no)
	(up-compare-goal gl-temp-seven g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-eight g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-seven g:< gl-temp-twenty)
	(up-compare-goal gl-temp-eight g:< gl-temp-twenty)
	;(up-compare-goal gl-temp-twelve < 7)
=>
	(up-lerp-tiles gl-temp-seven gl-temp-one c: -1)
	(up-modify-goal gl-temp-sixteen c:+ 1)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Cross X = %d " g: gl-temp-seven)
	; (up-chat-data-to-player my-player-number "Cross Y = %d " g: gl-temp-eight)
	; (up-chat-data-to-player my-player-number "Distance = %d " g: gl-temp-twelve)
	;(chat-to-all "Sixteen")
	;(up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	;(up-chat-data-to-player my-player-number "X = %d " g: gl-temp-nine)
	;(up-chat-data-to-player my-player-number "Y = %d " g: gl-temp-ten)
	(up-jump-rule -1)
)

; Getting closest unexplored lerp point towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-twelve > 0)
	(up-point-explored gl-temp-nine != explored-no)
	(up-compare-goal gl-temp-nine g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-ten g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-nine g:< gl-temp-twenty)
	(up-compare-goal gl-temp-ten g:< gl-temp-twenty)
	;(or	(up-point-explored gl-temp-nine == explored-active)
	;	(up-point-explored gl-temp-nine == explored-yes))
	;(up-compare-goal gl-temp-ten < 7)
=>
	;(up-set-target-point gl-temp-three)
	(up-lerp-tiles gl-temp-nine gl-temp-one c: -1)
	(up-modify-goal gl-temp-seventeen c:+ 1)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Lerp X = %d " g: gl-temp-three)
	; (up-chat-data-to-player my-player-number "Lerp Y = %d " g: gl-temp-four)
	; (up-chat-data-to-player my-player-number "Distance = %d " g: gl-temp-ten)
	;(chat-to-all "Towards TC")
	;(chat-to-all "Seventeen")
	(up-jump-rule -1)
)

; Getting closest unexplored in-between point towards TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-twelve > 0)
	(up-point-explored gl-temp-eleven != explored-no)
	(up-compare-goal gl-temp-eleven g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-twelve g:> gl-temp-nineteen)
	(up-compare-goal gl-temp-eleven g:< gl-temp-twenty)
	(up-compare-goal gl-temp-twelve g:< gl-temp-twenty)
	;(up-compare-goal gl-temp-twelve < 7)
=>
	(up-lerp-tiles gl-temp-eleven gl-temp-one c: -1)
	(up-modify-goal gl-temp-eighteen c:+ 1)
	; (up-chat-data-to-player my-player-number "Scout X = %d " g: gl-temp-one)
	; (up-chat-data-to-player my-player-number "Scout Y = %d " g: gl-temp-two)
	; (up-chat-data-to-player my-player-number "Cross X = %d " g: gl-temp-seven)
	; (up-chat-data-to-player my-player-number "Cross Y = %d " g: gl-temp-eight)
	; (up-chat-data-to-player my-player-number "Distance = %d " g: gl-temp-twelve)
	;(chat-to-all "Eighteen")
	(up-jump-rule -1)
)

; Going to TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-eight > 0)
	(up-compare-goal gl-temp-seventeen < 7)
	;(up-compare-goal gl-temp-sixteen g:<= gl-temp-fifteen)
=>
	(up-target-point gl-temp-nine action-default -1 -1)
	;(chat-to-all "To TC")
)

; Going cross
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-eight > 0)
	(or	(and	(up-compare-goal gl-temp-seventeen > 6)
				(up-compare-goal gl-temp-eighteen < 7))
		(and	(and	(and	(up-compare-goal gl-temp-fifteen < 7)
						(up-compare-goal gl-temp-sixteen > 6))
				(up-compare-goal gl-temp-seventeen > 6))
		(up-compare-goal gl-temp-eighteen > 6)))
=>
	(up-target-point gl-temp-five action-default -1 -1)
	;(chat-to-all "Cross")
)

; Going away from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-eight > 0)
	(up-compare-goal gl-temp-fourteen < 7)
	(up-compare-goal gl-temp-fifteen > 6)
	(up-compare-goal gl-temp-sixteen > 6)
	(up-compare-goal gl-temp-seventeen > 6)
	(up-compare-goal gl-temp-eighteen > 6)
=>
	(up-target-point gl-temp-three action-default -1 -1)
	;(chat-to-all "Away from TC")
)

; Going away+cross from TC
(defrule
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-scouting-direction != 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-scouting-direction == -1)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-temp-one > 0)
	(up-compare-goal gl-temp-eight > 0)
	(up-compare-goal gl-temp-sixteen < 7)
	(up-compare-goal gl-temp-seventeen > 6)
	(up-compare-goal gl-temp-eighteen > 6)
=>
	(up-target-point gl-temp-seven action-default -1 -1)
	;(chat-to-all "Away from TC - in-between")
)
;endregion

;region Sheep scouting - control group 8 - to be improved
(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
=>
	(up-full-reset-search)
	(up-find-local c: livestock-class c: 8)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
	(up-compare-goal gl-local-total > 2)
=>
	(up-set-target-point gl-starting-tc-x)
	(up-clean-search search-local object-data-distance search-order-asc)
	(up-remove-objects search-local object-data-index < 2)
	(up-modify-group-flag 0 c: 8)
	(up-reset-group c: 8)
	(up-create-group 0 0 c: 8)
	(up-modify-group-flag 1 c: 8)
	;(chat-to-all "Two")
)

(defrule
	(current-age == dark-age)
	(game-time >= 300)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
=>
	(up-full-reset-search)
	(up-modify-group-flag 0 c: 8)
	(up-reset-group c: 8)
	(up-create-group 0 0 c: 8)
	(up-modify-group-flag 1 c: 8)
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-compare-goal gl-starting-tc-x > 0)
	(up-compare-goal gl-starting-tc-y > 0)
	(up-group-size c: 8 > 0)
=>
	(up-full-reset-search)
	(up-modify-goal gl-temp-three c: 0)
	;(chat-to-all "Three")
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
=>
	(up-set-group search-local c: 8)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Four")
	;(up-chat-data-to-player my-player-number "gl-local-total = %d " g: gl-local-total)
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local g: gl-temp-three)
	(up-compare-goal gl-temp-three g:< gl-local-total)
	(up-compare-goal gl-scouting-direction == -1)
=>
	(up-set-group search-local c: 8)
	(up-remove-objects search-local object-data-index g:!= gl-temp-three)
	(up-get-object-data object-data-point-x  gl-temp-one)
	;(up-chat-data-to-player my-player-number "X = %d " g: gl-temp-one)
	(up-get-object-data object-data-point-y  gl-temp-two)
	;(up-chat-data-to-player my-player-number "Y = %d " g: gl-temp-two)
	(up-cross-tiles gl-temp-one gl-starting-tc-x c: 9)
	(up-bound-precise-point gl-temp-one 0 c: 2)
	;(chat-to-all "Five")
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
	(up-set-target-object search-local g: gl-temp-three)
	(up-compare-goal gl-temp-three g:< gl-local-total)
	(up-compare-goal gl-scouting-direction == 1)
=>
	(up-set-group search-local c: 8)
	(up-remove-objects search-local object-data-index g:!= gl-temp-three)
	(up-get-object-data object-data-point-x  gl-temp-one)
	;(up-chat-data-to-player my-player-number "X = %d " g: gl-temp-one)
	(up-get-object-data object-data-point-y  gl-temp-two)
	;(up-chat-data-to-player my-player-number "Y = %d " g: gl-temp-two)
	(up-cross-tiles gl-temp-one gl-starting-tc-x c: -9)
	(up-bound-precise-point gl-temp-one 0 c: 2)
	;(chat-to-all "Five")
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
	(up-point-explored gl-temp-one == explored-no)
	(up-compare-goal gl-temp-three g:< gl-local-total)
=>
	;(up-chat-data-to-player my-player-number "New X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "New Y = %d " g: gl-temp-two)
	(up-lerp-tiles gl-temp-one gl-starting-tc-x c: 1)
	;(chat-to-all "Six")
	(up-jump-rule -1)
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
	(up-point-explored gl-temp-one != explored-no)
	(up-compare-goal gl-temp-three g:< gl-local-total)
=>
	;(up-chat-data-to-player my-player-number "New X = %d " g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "New Y = %d " g: gl-temp-two)
	(up-lerp-tiles gl-temp-one gl-starting-tc-x c: -4)
	;(up-bound-point gl-temp-one gl-temp-one)
	(up-bound-precise-point gl-temp-one 0 c: 2)
	(up-target-point gl-temp-one action-default -1 -1)
	;(chat-to-all "Seven")
)

(defrule
	(current-age == dark-age)
	(game-time < 300)
	(up-group-size c: 8 > 0)
	(timer-triggered three-second-timer)
	(up-compare-goal gl-temp-three g:< gl-local-total)
=>
	(up-modify-goal gl-temp-three c:+ 1)
	;(chat-to-all "Eight")
	(up-jump-rule -6)
)
;endregion

;region Getting scout ID
(defrule
	(game-time > 2)
=>
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	(disable-self)
)

(defrule
	(game-time > 2)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-id gl-scout-id-one)
	;(up-chat-data-to-player my-player-number "gl-scout-id-one = %d " g: gl-scout-id-one)
	(disable-self)
)
;endregion

;region Collecting last sheep, if any are left
(defrule
	(up-compare-goal gl-scouting-goal == 0)
	(up-compare-goal gl-dlure == 100)
	(current-age == dark-age)
	(game-time > 240)
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

(defrule
	(up-compare-goal gl-scouting-goal == 0)
	(up-compare-goal gl-dlure == 100)
	(current-age == dark-age)
	(game-time > 240)
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-set-target-object search-local c: 0)
=>
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(set-strategic-number sn-focus-player-number 0)
	(up-find-remote c: livestock-class c: 4)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Two")
)

#load-if-not-defined UP-ALLY-IN-GAME
(defrule
	(up-compare-goal gl-scouting-goal == 0)
	(up-compare-goal gl-dlure == 100)
	(current-age == dark-age)
	(game-time > 240)
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-game-stage < 1)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-set-target-point gl-temp-one)
	(up-clean-search search-remote object-data-distance search-order-asc)
	(up-set-target-object search-remote c: 0)
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Three")
	(up-jump-rule 37)
)
#end-if

#load-if-defined UP-ALLY-IN-GAME
(defrule
	(up-compare-goal gl-scouting-goal == 0)
	(up-compare-goal gl-dlure == 100)
	(current-age == dark-age)
	(game-time > 240)
	(up-group-size c: 9 > 0)
	(up-compare-goal gl-local-total > 0)
	(up-set-target-object search-remote c: 0)
=>
	(up-set-target-point gl-temp-one)
	(up-clean-search search-remote object-data-distance search-order-asc)
	(up-set-target-object search-remote c: 0)
	(up-target-objects 1 action-default -1 -1)
	;(chat-to-all "Three")
	(up-jump-rule 11)
)
#end-if
;endregion

;region Looking for opponent's base
;1v1
#load-if-not-defined UP-ALLY-IN-GAME
(defrule
	(up-compare-goal gl-dlure != 100)
	(game-time > 480)
=>
	(up-modify-goal gl-dlure c:= 100)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
=>
	;(chat-to-all "One")
	(up-modify-goal gl-game-stage c:= 1)
	(up-get-point position-corner gl-target-one-x)
	(up-bound-precise-point gl-target-one-x 0 c: 18) ; Moving gl-target-one-x away from corner by x=18, y=18
	;(up-get-point position-corner gl-target-one-x)
	;(up-get-point position-center gl-center-x)
	;(up-chat-data-to-player my-player-number "gl-target-one-x = %d " g: gl-target-one-x)
	;(up-chat-data-to-player my-player-number "gl-target-one-y = %d " g: gl-target-one-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	;(up-chat-data-to-player my-player-number "New gl-target-one-x = %d " g: gl-target-one-x)
	;(up-chat-data-to-player my-player-number "New gl-target-one-y = %d " g: gl-target-one-y)
	(up-get-point position-map-size gl-temp-x)
	(up-modify-goal gl-temp-x c:/ 2)
	(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	(up-modify-goal gl-temp-one g:= gl-temp-x)
	(up-modify-goal gl-temp-one c:- 18)
	(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-lerp-tiles gl-target-one-x gl-temp-x g: gl-temp-one)
	;(up-chat-data-to-player my-player-number "New gl-target-one-x = %d " g: gl-target-one-x)
	;(up-chat-data-to-player my-player-number "New gl-target-one-y = %d " g: gl-target-one-y)
	(up-modify-goal gl-scouting-goal c:= 1)
	;(chat-to-all "Old rule")
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 1)
=>
	;(chat-to-all "Two")
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	;(chat-to-all "One")
	(disable-self)
)
	
(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 1)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Two")
	;(chat-to-all "Three")
	(up-target-point gl-target-one-x action-default -1 -1)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 1)
=>
	(up-full-reset-search)
	;(chat-to-all "Four")
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 1)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Five")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 1)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	;(chat-to-all "Yes")
	;(chat-to-all "Six")
	(up-modify-goal gl-game-stage c:= 2)
	(up-get-point position-map-size gl-temp-x)
	(up-modify-goal gl-temp-x g:= gl-target-one-x)
	(up-modify-goal gl-temp-y c:/ 2)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	(up-modify-goal gl-temp-one g:= gl-temp-y)
	(up-modify-goal gl-temp-one c:- 18)
	(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-lerp-tiles gl-target-one-x gl-temp-x g: gl-temp-one)
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	;(chat-to-all "Three")
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 2)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Four")
	;(chat-to-all "Seven")
	(up-target-point gl-target-one-x action-default -1 -1)
	(up-modify-goal gl-temp-one c:= 0)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 2)
=>
	;(chat-to-all "Eight")
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 2)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Nine")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 2)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	;(chat-to-all "Ten")
	(up-modify-goal gl-game-stage c:= 3)
	(up-get-point position-map-size gl-temp-x)
	(up-modify-goal gl-temp-x c:/ 2)
	(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	(up-modify-goal gl-temp-one g:= gl-temp-x)
	(up-modify-goal gl-temp-one c:- 18)
	(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-lerp-tiles gl-target-one-x gl-temp-x g: gl-temp-one)
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	;(chat-to-all "Five")
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 3)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Six")
	;(chat-to-all "Eleven")
	(up-target-point gl-target-one-x action-default -1 -1)
	(up-modify-goal gl-temp-one c:= 0)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 3)
=>
	(up-full-reset-search)
	;(chat-to-all "Twelve")
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 3)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Thirteen")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 3)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	(up-modify-goal gl-game-stage c:= 4)
	;(chat-to-all "Fourteen")
	;(up-get-point position-map-size gl-temp-x)
	;(up-modify-goal gl-temp-x c:/ 2)
	;(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	;(up-modify-goal gl-temp-one g:= gl-temp-x)
	;(up-modify-goal gl-temp-one c:- 13)
	;(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-get-point position-corner gl-temp-x)
	(up-bound-precise-point gl-temp-x 0 c: 18)
	(up-lerp-tiles gl-target-one-x gl-temp-x c: 21)
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	;(chat-to-all "Five")
	(disable-self)
)
	
(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 4)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Six")
	;(chat-to-all "Fifteen")
	(up-target-point gl-target-one-x action-default -1 -1)
	(up-modify-goal gl-temp-one c:= 0)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 4)
=>
	(up-full-reset-search)
	;(chat-to-all "Sixteen")
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 4)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Seventeen")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 4)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	(up-modify-goal gl-game-stage c:= 5)
	;(chat-to-all "Eighteen")
	;(up-get-point position-map-size gl-temp-x)
	;(up-modify-goal gl-temp-x c:/ 2)
	;(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	;(up-modify-goal gl-temp-one g:= gl-temp-x)
	;(up-modify-goal gl-temp-one c:- 13)
	;(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	
	(up-get-point position-map-size gl-temp-x)
	(up-modify-goal gl-temp-x c:/ 2)
	(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	(up-modify-goal gl-temp-one g:= gl-temp-x)
	(up-modify-goal gl-temp-one c:- 18)
	(up-modify-goal gl-temp-one c:* 2)
	(up-modify-goal gl-temp-one c:- 18)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-lerp-tiles gl-target-one-x gl-temp-x g: gl-temp-one)
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	; (up-get-point position-corner gl-temp-x)
	; (up-bound-precise-point gl-temp-x 0 c: 13)
	; (up-lerp-tiles gl-target-one-x gl-temp-x c: 18)
	; (up-full-reset-search)
	; (up-find-local c: scout-type c: 1)
	;(chat-to-all "Five")
	(disable-self)
)
	
(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 5)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Six")
	;(chat-to-all "Nineteen")
	(up-target-point gl-target-one-x action-default -1 -1)
	(up-modify-goal gl-temp-one c:= 0)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 5)
=>
	(up-full-reset-search)
	;(chat-to-all "Twenty")
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 5)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Twenty one")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 5)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	(up-modify-goal gl-game-stage c:= 6)
	;(chat-to-all "Twenty two")
	;(up-get-point position-map-size gl-temp-x)
	;(up-modify-goal gl-temp-x c:/ 2)
	;(up-modify-goal gl-temp-y g:= gl-target-one-y)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	;(up-modify-goal gl-temp-one g:= gl-temp-x)
	;(up-modify-goal gl-temp-one c:- 13)
	;(up-modify-goal gl-temp-one c:* 2)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	
	(up-get-point position-map-size gl-temp-x)
	(up-modify-goal gl-temp-x g:= gl-target-one-x)
	(up-modify-goal gl-temp-y c:/ 2)
	;(up-chat-data-to-player my-player-number "gl-temp-x = %d " g: gl-temp-x)
	;(up-chat-data-to-player my-player-number "gl-temp-y = %d " g: gl-temp-y)
	;(up-lerp-tiles gl-target-one-x gl-center-x c: 18)
	(up-modify-goal gl-temp-one g:= gl-temp-y)
	(up-modify-goal gl-temp-one c:- 18)
	(up-modify-goal gl-temp-one c:* 2)
	(up-modify-goal gl-temp-one c:- 36)
	;(up-chat-data-to-player my-player-number "gl-temp-one = %d " g: gl-temp-one)
	(up-lerp-tiles gl-target-one-x gl-temp-x g: gl-temp-one)
	(up-full-reset-search)
	(up-find-local c: scout-type c: 1)
	; (up-get-point position-corner gl-temp-x)
	; (up-bound-precise-point gl-temp-x 0 c: 13)
	; (up-lerp-tiles gl-target-one-x gl-temp-x c: 18)
	; (up-full-reset-search)
	; (up-find-local c: scout-type c: 1)
	;(chat-to-all "Five")
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 6)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Six")
	;(chat-to-all "Twenty three")
	(up-target-point gl-target-one-x action-default -1 -1)
	(up-modify-goal gl-temp-one c:= 0)
	(disable-self)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 6)
=>
	(up-full-reset-search)
	;(chat-to-all "Twenty four")
	(up-find-local c: scout-type c: 1)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 6)
	(up-set-target-object search-local c: 0)
=>
	;(chat-to-all "Twenty five")
	(up-set-target-point gl-target-one-x)
	(up-get-object-data object-data-distance gl-temp-one)
)

(defrule
	(up-compare-goal gl-dlure == 100)
	(game-time > 240)
	(up-compare-goal gl-game-stage == 6)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-one < 8)
=>
	(up-modify-goal gl-game-stage c:= 10)
	;(chat-to-all "Done")
	(disable-self)
)
#end-if

;Team games
#load-if-defined UP-ALLY-IN-GAME
(defrule
	(or	(and	(current-age == feudal-age)
				(up-compare-goal gl-dlure != 100))
		(and	(up-compare-goal gl-dlure == 100)
				(game-time > 240)))
=>
	(up-modify-goal gl-dlure  c:= 100)	
	(up-modify-goal gl-game-stage c:= 1)
	(up-send-scout group-type-land-explore scout-flank)
	;(chat-to-all "New rule")
	(up-get-point position-flank gl-temp-one)
	;(up-send-flare gl-temp-one)
	(disable-self)
)
#end-if
;endregion

;region Finding enemy buildings
;Setting sn-focus-player-number to 1 so search can start
(defrule
	(up-compare-goal gl-game-stage < 10)
	(up-compare-goal gl-scout-id-one > 0)
	(players-building-count any-enemy > 0)
	(timer-triggered three-second-timer)
	;(timer-triggered t-one-second)
=>
	(set-strategic-number sn-focus-player-number 1)
	;(up-chat-data-to-player my-player-number "my-player-number = %d " c: my-player-number)
	;(up-full-reset-search)
)

;Looking for enemy buildings around scout
(defrule
	(up-compare-goal gl-game-stage < 10)
	(up-compare-goal gl-scout-id-one > 0)
	(players-building-count any-enemy > 0)
	(timer-triggered three-second-timer)
	(player-in-game focus-player)
	(stance-toward focus-player enemy)
	;(timer-triggered t-one-second)
=>
	(up-full-reset-search)
	(up-set-target-by-id g: gl-scout-id-one)
	(up-get-object-data object-data-point-x gl-temp-x)
	(up-get-object-data object-data-point-y gl-temp-y)
	(up-set-target-point gl-temp-x)
	(up-reset-filters)
	(up-filter-distance c: -1 c: 10)
	(up-find-remote c: town-center c: 1)
	(up-find-remote c: house c: 1)
	(up-find-remote c: lumber-camp c: 1)
	(up-find-remote c: mill c: 1)
	(up-find-remote c: mining-camp c: 1)
	(up-get-search-state gl-local-total)
)

;Changing sn-focus-player-number if enemy was found
(defrule
	(up-compare-goal gl-game-stage < 10)
	(up-compare-goal gl-scout-id-one > 0)
	(players-building-count any-enemy > 0)
	(timer-triggered three-second-timer)
	(player-in-game focus-player)
	(stance-toward focus-player enemy)
	(up-compare-goal gl-remote-total > 0)
	;(timer-triggered t-one-second)
=>
	;(up-modify-goal gl-game-stage c:= 10)
	(up-modify-goal gl-enemy-number s:= sn-focus-player-number)
	;(up-chat-data-to-player my-player-number "Enemy player number g = %d " g: gl-enemy-number)
	;(up-chat-data-to-player my-player-number "Enemy player number = %d " s: sn-focus-player-number)
	;(up-chat-data-to-player my-player-number "Enemy buildings found = %d " g: gl-remote-total)
	;(chat-to-all "Rule 1")
)

;If no enemy buildings were found, increasing sn-focus-player-number by 1 and up-jump so search can be repeated
(defrule
	(up-compare-goal gl-game-stage < 10)
	(up-compare-goal gl-scout-id-one > 0)
	(players-building-count any-enemy > 0)
	(timer-triggered three-second-timer)
	(up-compare-sn sn-focus-player-number < 8)
	;(timer-triggered t-one-second)
=>
	(up-modify-sn sn-focus-player-number c:+ 1)
	;(up-chat-data-to-player my-player-number "Player number checked = %d " s: sn-focus-player-number)
	(up-jump-rule -3)
)

;If enemy TC was found, getting that TC into remote list
(defrule
	(timer-triggered three-second-timer)
	(players-building-type-count any-enemy town-center > 0)
	;(up-compare-goal gl-game-stage >= 10)
	(up-compare-goal gl-game-stage <= 11)
	(up-compare-goal gl-scout-id-one > 0)
	(up-compare-goal gl-enemy-tc-y < 1)
	;(up-compare-goal gl-enemy-number > 0)
	;(timer-triggered t-one-second)
=>
	(up-full-reset-search)
	(up-modify-sn sn-focus-player-number g:= gl-enemy-number)
	(up-find-remote c: town-center c: 1)
	;(disable-self)
	;(chat-to-all "Rule 2")
)

;If TC is in remote list, getting its x/y coordinates
(defrule
	(timer-triggered three-second-timer)
	(players-building-type-count any-enemy town-center > 0)
	;(up-compare-goal gl-game-stage >= 10)
	(up-compare-goal gl-game-stage <= 11)
	(up-set-target-object search-remote c: 0)
	(up-compare-goal gl-scout-id-one > 0)
	(up-compare-goal gl-enemy-tc-y < 1)
	;(timer-triggered t-one-second)
=>
	(up-get-object-data object-data-point-x gl-enemy-tc-x)
	(up-get-object-data object-data-point-y gl-enemy-tc-y)
	;(disable-self)
	;(up-modify-goal gl-game-stage c:= 11)
	;(chat-to-all "Enemy TC found")
	;(up-jump-rule 1)
)

;If no TC was found, looking for houses etc.
(defrule
	(timer-triggered three-second-timer)
	(players-building-count any-enemy > 0)
	;(up-compare-goal gl-game-stage == 10)
	(up-compare-goal gl-game-stage <= 11)
	(up-compare-goal gl-scout-id-one > 0)
	(up-compare-goal gl-enemy-tc-y < 1)
	;(timer-triggered t-one-second)
=>
	(up-full-reset-search)
	(up-modify-sn sn-focus-player-number g:= gl-enemy-number)
	(up-find-remote c: house c: 1)
	(up-find-remote c: lumber-camp c: 1)
	(up-find-remote c: mill c: 1)
	(up-find-remote c: mining-camp c: 1)
	;(up-get-search-state gl-local-total)
	;(chat-to-all "Rule 4")
)

;If any building was found, getting its x/y coordinates
(defrule
	(timer-triggered three-second-timer)
	(players-building-count any-enemy > 0)
	(up-set-target-object search-remote c: 0)
	;(up-compare-goal gl-game-stage >= 10)
	(up-compare-goal gl-game-stage <= 11)
	(up-compare-goal gl-enemy-tc-y < 1)
	;(timer-triggered t-one-second)
=>
	(up-get-object-data object-data-point-x gl-temp-x); building x coordinate
	(up-get-object-data object-data-point-y gl-temp-y); building y coordinate
	(up-add-object-by-id search-local g: gl-scout-id-one)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Rule 5")
)

;Cross circle around found building
(defrule
	(timer-triggered three-second-timer)
	(players-building-count any-enemy > 0)
	(up-set-target-object search-local c: 0)
	;(up-compare-goal gl-game-stage >= 10)
	(up-compare-goal gl-game-stage <= 11)
	(up-compare-goal gl-local-total > 0)
	(up-compare-goal gl-enemy-tc-y < 1)
	;(timer-triggered t-one-second)
=>
	(up-get-object-data object-data-point-x gl-target-one-x); scout x coordinate
	(up-get-object-data object-data-point-y gl-target-one-y); scout y coordinate
	(up-cross-tiles gl-target-one-x gl-temp-x c: 9)
	(up-modify-goal gl-temp-five g:= gl-temp-x)
	(up-modify-goal gl-temp-six g:= gl-temp-y)
	(up-lerp-tiles gl-temp-five gl-target-one-x c: 9)
	(up-modify-goal gl-target-one-x g:= gl-temp-five)
	(up-modify-goal gl-target-one-y g:= gl-temp-six)
	(up-bound-point gl-target-one-x gl-target-one-x)
	(up-target-point gl-target-one-x action-default -1 -1)
	;(chat-to-all "Rule 6")
)

;If enemy TC has been found, get scout to local list
(defrule
	(timer-triggered three-second-timer)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-group-size c: 9 > 0)
	(timer-triggered three-second-timer)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 9)
)

;Circle enemy TC
(defrule
	(timer-triggered three-second-timer)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-group-size c: 9 > 0)
	(up-set-target-object search-local c: 0)
	(timer-triggered three-second-timer)
=>
	(up-get-object-data object-data-point-x gl-temp-one)
	(up-get-object-data object-data-point-y gl-temp-two)
	(up-set-target-point gl-enemy-tc-x)
	(up-cross-tiles gl-temp-one gl-enemy-tc-x c: 9)
	(up-modify-goal gl-temp-three g:= gl-temp-one)
	(up-modify-goal gl-temp-four g:= gl-temp-two)
	(up-modify-goal gl-temp-one g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-two g:= gl-enemy-tc-y)
	(up-lerp-tiles gl-temp-one gl-temp-three c: 12)
	(up-target-point gl-temp-one action-default -1 -1)
)
;endregion

;region M@a control - to be improved
;<<<Creating control group
;Looking for 3 militia
(defrule
	(unit-type-count militia-line >= 3)
	(up-group-size c: 7 c:< 1)
=>
	(up-full-reset-search)
	(up-find-local c: militia-line c: 3)
	(up-get-search-state gl-local-total)
	;(chat-to-all "Looking for militia")
)

;Creating a control group 7
(defrule
	(unit-type-count militia-line >= 3)
	(up-group-size c: 7 c:< 1)
	(up-compare-goal gl-local-total == 3)
=>
	(up-modify-group-flag 0 c: 7)
	(up-reset-group c: 7)
	(up-create-group 0 0 c: 7)
	(up-modify-group-flag 1 c: 7)
	;(chat-to-all "Creating group seven")
)

;<<<Sending militia forward if no TC has been found yet
;Selecting control group
(defrule
	(up-group-size c: 7 c:> 1)
	(building-type-count town-center > 0)
	(up-compare-goal gl-enemy-tc-x < 1)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 7)
	(up-get-point position-opposite gl-temp-one)
	;(chat-to-all "Selecting group 7")
	(disable-self)
)

;Sending group to position-opposite
(defrule
	(up-group-size c: 7 c:> 1)
	(building-type-count town-center > 0)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-enemy-tc-x < 1)
=>
	(up-target-point gl-temp-one action-default -1 -1);stance-defensive)
	;(chat-to-all "Attack!")
	(disable-self)
)

;<<<Sending militia forward if no TC has been found yet
(defrule
	(up-group-size c: 7 c:> 1)
	(up-compare-goal gl-enemy-tc-x > 0)
=>
	(up-full-reset-search)
	(up-set-group search-local c: 7)
	;(chat-to-all "Selecting group 7")
	(disable-self)
)

(defrule
	(up-group-size c: 7 c:> 1)
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-set-target-object search-local c: 0)
=>
	(up-set-target-point gl-enemy-tc-x)
	;(up-target-point gl-enemy-tc-x action-patrol -1 stance-defensive)
	(up-target-point gl-enemy-tc-x action-default -1 -1)
	;(chat-to-all "Attack - TC")
	(disable-self)
)
;endregion

;region Simple forward gather point for Barracks
(defrule
	(up-group-size c: 7 c:> 0)
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(building-type-count barracks > 0)
=>
	(up-full-reset-search)
	(up-find-local c: barracks c: 1)
	;(chat-to-all "Barracks search")
	(disable-self)
)

(defrule
	(up-group-size c: 7 c:> 0)
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-starting-tc-x > 0)
	(building-type-count barracks > 0)
	(up-set-target-object search-local c: 0)
=>
	(up-modify-goal gl-temp-one g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-two g:= gl-enemy-tc-y)
	(up-lerp-tiles gl-temp-one gl-starting-tc-x c: 10)
	(up-target-point gl-temp-one action-gather -1 -1)
	;(chat-to-all "Barracks gather point")
	(disable-self)
)
;endregion

;region Enemy TC avoidance
;Defining enemy TC borders

;Rough search around enemy TC for militia-line, skirmisher-line and spearman-line units
(defrule
	;(up-set-target-object search-local c: 0)
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
=>
	(up-full-reset-search)
	(up-set-target-point gl-enemy-tc-x)
	(up-filter-distance c: -1 c: 10)
	(up-modify-sn sn-focus-player-number c:= my-player-number)
	(up-find-remote c: militia-line c: 20)
	(up-find-remote c: skirmisher-line c: 20)
	(up-find-remote c: spearman-line c: 20)
	(up-get-search-state gl-local-total)
	;(chat-to-all "One")
)

;If any units are around enemy TC, getting needed x/y coordinates of enemy TC range - goals 1-8
(defrule
	;(up-set-target-object search-local c: 0)
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
=>
	;(up-get-object-data object-data-point-x gl-temp-one)
	(up-modify-goal gl-temp-one g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-two g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-three g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-four g:= gl-enemy-tc-x)
	(up-modify-goal gl-temp-one c:- 8)
	(up-modify-goal gl-temp-two c:- 2)
	(up-modify-goal gl-temp-three c:+ 1)
	(up-modify-goal gl-temp-four c:+ 7)
	
	;(up-get-object-data object-data-point-y gl-enemy-tc-y)
	(up-modify-goal gl-temp-five g:= gl-enemy-tc-y)
	(up-modify-goal gl-temp-six g:= gl-enemy-tc-y)
	(up-modify-goal gl-temp-seven g:= gl-enemy-tc-y)
	(up-modify-goal gl-temp-eight g:= gl-enemy-tc-y)
	(up-modify-goal gl-temp-five c:- 8)
	(up-modify-goal gl-temp-six c:- 2)
	(up-modify-goal gl-temp-seven c:+ 1)
	(up-modify-goal gl-temp-eight c:+ 7)
	;(up-full-reset-search)
	;(up-find-local c: scout-cavalry-line c: 1)
	;(chat-to-all "X/Ys and scout search")
	(up-modify-goal gl-temp-eleven c:= 0);In range of TC
	(up-modify-goal gl-temp-thirteen c:= 0);Indexer
	;(chat-to-all "Two")
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
=>
	(up-reset-search 1 1 0 0)
	(up-get-object-data object-data-id gl-temp-sixteen)
	(up-add-object-by-id search-local g: gl-temp-sixteen)
	(up-get-object-data object-data-point-x gl-temp-nine);unit X coordinate
	(up-get-object-data object-data-point-y gl-temp-ten);unit Y coordinate
	;(chat-to-all "Three")
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(or	(and(up-compare-goal gl-temp-nine g:>= gl-temp-one)
			(and(up-compare-goal gl-temp-nine g:<= gl-temp-four)
				(and(up-compare-goal gl-temp-ten g:>= gl-temp-six)
					(up-compare-goal gl-temp-ten g:<= gl-temp-seven))))
		(and(up-compare-goal gl-temp-nine g:>= gl-temp-two)
			(and(up-compare-goal gl-temp-nine g:<= gl-temp-three)
				(and(up-compare-goal gl-temp-ten g:>= gl-temp-five)
					(up-compare-goal gl-temp-ten g:<= gl-temp-eight)))))
=>
	(up-modify-goal gl-temp-eleven c:= 1)
	;(chat-to-all "In range of TC")
)

;Left corner
(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
=>
	(up-modify-goal gl-temp-fourteen g:= gl-temp-two)
	(up-modify-goal gl-temp-fifteen g:= gl-temp-six)
	(up-set-target-point gl-temp-fourteen)
	(up-get-object-data object-data-precise-distance gl-temp-twelve)
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
	(up-compare-goal gl-temp-twelve <= 650)
=>
	(up-modify-goal gl-temp-eleven c:= 1)
	;(chat-to-all "Left corner")
)

;Right corner
(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
=>
	(up-modify-goal gl-temp-fourteen g:= gl-temp-three)
	(up-modify-goal gl-temp-fourteen c:+ 1)
	(up-modify-goal gl-temp-fifteen g:= gl-temp-seven)
	(up-modify-goal gl-temp-fifteen c:+ 1)
	(up-set-target-point gl-temp-fourteen)
	(up-get-object-data object-data-precise-distance gl-temp-twelve)
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
	(up-compare-goal gl-temp-twelve <= 650)
=>
	(up-modify-goal gl-temp-eleven c:= 1)
	;(chat-to-all "Right corner")
)

;Lower corner
(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
=>
	(up-modify-goal gl-temp-fourteen g:= gl-temp-two)
	(up-modify-goal gl-temp-fifteen g:= gl-temp-seven)
	(up-modify-goal gl-temp-fifteen c:+ 1)
	(up-set-target-point gl-temp-fourteen)
	(up-get-object-data object-data-precise-distance gl-temp-twelve)
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
	(up-compare-goal gl-temp-twelve <= 650)
=>
	(up-modify-goal gl-temp-eleven c:= 1)
	;(chat-to-all "Lower corner")
)

;Upper corner
(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
=>
	(up-modify-goal gl-temp-fourteen g:= gl-temp-three)
	(up-modify-goal gl-temp-fourteen c:+ 1)
	(up-modify-goal gl-temp-fifteen g:= gl-temp-six)
	(up-set-target-point gl-temp-fourteen)
	(up-get-object-data object-data-precise-distance gl-temp-twelve)
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven != 1)
	(up-compare-goal gl-temp-twelve <= 650)
=>
	(up-modify-goal gl-temp-eleven c:= 1)
	;(chat-to-all "Upper corner")
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-local c: 0)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
	(up-compare-goal gl-temp-eleven == 1)
=>
	(up-lerp-tiles gl-temp-nine gl-enemy-tc-x c: -1)
	(up-cross-tiles gl-temp-nine gl-enemy-tc-x c: 4)
	(up-target-point gl-temp-nine action-default -1 -1)
	;(chat-to-all "Move back")
)

(defrule
	(up-compare-goal gl-enemy-tc-x > 0)
	(up-compare-goal gl-enemy-tc-y > 0)
	(up-compare-goal gl-remote-total > 0)
	(up-set-target-object search-remote g: gl-temp-thirteen)
	(up-compare-goal gl-temp-thirteen g:< gl-remote-total)
=>
	(up-modify-goal gl-temp-thirteen c:+ 1)
	(up-modify-goal gl-temp-eleven c:= 0)
	(up-jump-rule -12)
)
;endregion



;<<< Resetting timers >>>
;region Three seconds timer reset
(defrule
	(timer-triggered three-second-timer)
=>
	(enable-timer three-second-timer 3)
	;(chat-to-all "Three seconds")
)
;endregion

;region Ten seconds timer reset
(defrule
	(current-age >= feudal-age)
	(timer-triggered ten-seconds-timer)
=>
	(enable-timer ten-seconds-timer 10)
	;(chat-to-all "Three seconds")
)
;endregion

;region One minute timer reset
(defrule
	(current-age >= feudal-age)
	(timer-triggered one-minute-timer)
=>
	(enable-timer one-minute-timer 60)
)
;endregion

	;(chat-to-all "Something something")
	;(up-chat-data-to-player my-player-number "The smallest value is = %d " g: gl-distance-two-three)




