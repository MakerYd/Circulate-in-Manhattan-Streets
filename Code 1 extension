globals [
  taille-bloc
  les-rues
  les-carrefours
  redV
  greenV
  redH
  greenH
]

breed [ vehicules vehicule ]
breed[houses house]



breed[lightsL lightL]

breed[lightsD lightD]
breed[persons person]

vehicules-own [
  carrefour-depart
  carrefour-arrivee
  temps-parcours

]



to setup
  clear-all
  set taille-bloc 30

  ask patches [set pcolor black] ; initialement tous les patches sont noirs
  set les-rues patches with [ pycor mod taille-bloc = 0 or pxcor mod taille-bloc = 0 or pycor mod taille-bloc = 29 or pxcor mod taille-bloc = 29  ]
  ask les-rues [set pcolor pink] ;; afficher les rues en blanc
  set les-carrefours patches with [ pxcor mod taille-bloc = 0 and pycor mod taille-bloc = 0 ]
  ask les-carrefours [set pcolor yellow] ;; afficher les carrefours en jaune
  draw-houses
  place-lights
  place-people
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


end

to go
  ;; ask vehicules [move-random]
  ;;ask vehicules [ move-random-avec-arrivee ]
  ask vehicules [ move-random ]
  control-traffic-lights
  tick
end






to control-traffic-lights
  if ticks mod (50 * lights-interval * greenH + 65 * lights-interval * redH ) = 0 [change-color lightsL "H" change-color lightsD "H"]

end


to change-color [lights D]

  ask one-of lights [
    ifelse color = red [
      ifelse D = "H" [
        set greenH greenH + 1
        ][
        set greenV greenV + 1]
        ]
    [
      ifelse D = "H" [
        set redH redH + 1][
        set redV redV + 1]
        ]

  ]

  ask lights [
    ifelse color = red [set color green][set color red]
  ]
end


to place-lights
 ask patches with [pxcor mod taille-bloc = 29 and pycor mod taille-bloc = 1 ] [
    sprout-lightsL 1 [
      set color red
      set shape "square"
      set size 2
    ]
  ]


  ask patches with [pxcor mod taille-bloc = 1 and pycor mod taille-bloc = 0] [
    sprout-lightsD 1 [
      set color green
      set shape "square"
      set size 2
    ]
  ]

  set greenH 0
  set redH 1
  set redV 0
  set greenV 1

end


to place-people
  ask patches with [pcolor = black] [
    if count neighbors with [pcolor = black] = 8 and not any? turtles in-radius 7 [
      sprout-persons 1 [

        set size 2

        set shape  "person"
        ]
      ]
    ]

end


to draw-houses
  ask patches with [pcolor = black] [
    if count neighbors with [pcolor = black] = 8 and not any? turtles in-radius 10 [
       sprout-houses 1   [ ;; créer un véhicule sur le patch-carrefour
        set shape "house"
        set size 3
        stamp
      ]
    ]
  ]

  ask houses [die]


end



to move-random ;; procédure exécutée par chaque véhicule
     if ( patch-here = carrefour-arrivee   ) [
       set size 7
     ]
     if ( pcolor = yellow ) [ ;; véhicule sur un carrefour

        face one-of neighbors4
     ]
     forward 1
     set temps-parcours temps-parcours + 1
end
