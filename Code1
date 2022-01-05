globals [
  taille-bloc
  les-rues
  les-carrefours
]

breed [ vehicules vehicule ]

vehicules-own [ carrefour-depart carrefour-arrivee temps-parcours]

to setup
  clear-all
  set taille-bloc 7
  ask patches [set pcolor black] ; initialement tous les patches sont noirs
  set les-rues patches with [ pycor mod taille-bloc = 0 or pxcor mod taille-bloc = 0 ]
  ask les-rues [set pcolor white] ;; afficher les rues en blanc
  set les-carrefours patches with [ pxcor mod taille-bloc = 0 and pycor mod taille-bloc = 0 ]
  ask les-carrefours [set pcolor yellow] ;; afficher les carrefours en jaune
  setup-vehicules
  reset-ticks
end

to setup-vehicules
   let nb-vehicules round (densite-vehicules * count les-carrefours)
  ask n-of nb-vehicules les-carrefours [
       sprout-vehicules 1 [ ;; créer un véhicule sur le patch-carrefour
       set carrefour-depart patch-here
       set carrefour-arrivee one-of les-carrefours
           set shape "car"
           set size 4
           pen-up
       ]
   ]
   ask vehicule 0 [
       set color red
       set shape "truck"
       pen-down
   ]
end

to go
  ;; ask vehicules [move-random]
  ;;ask vehicules [ move-random-avec-arrivee ]
  ask vehicules [ move-optimise ]
  tick
end

to move-random ;; procédure exécutée par chaque véhicule
     if ( pcolor = yellow ) [ ;; véhicule sur un carrefour
        face one-of neighbors4
     ]
     forward 1
     set temps-parcours temps-parcours + 1
end

to move-random-avec-arrivee ;; procédure exécutée par chaque véhicule
   ifelse ( patch-here  = carrefour-arrivee)
       [ set size 8 ]
       [  ;; véhicule pas encore arrivé
          move-random
       ]
end

to move-optimise ;; procédure exécutée par chaque véhicule
   ifelse ( patch-here = carrefour-arrivee)
      [set size 8]
      [ ;; vehicule pas encore arrivé
          if ( pcolor = yellow ) [ ;; véhicule sur un carrefour
              let but carrefour-arrivee
      face min-one-of neighbors4 [distance but]
          ]
          forward 1
          set temps-parcours temps-parcours + 1
      ]
end
