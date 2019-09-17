# Sonic Pi
# Et si nous reproduisions le thème de Super Mario ?

## Installation de Sonic Pi

Vous pouvez installer Sonic Pi en vous basant sur les instructions données sur le site https://sonic-pi.net/

Pour l'installation sous Linux, voici la documentation : https://github.com/samaaron/sonic-pi/blob/master/INSTALL-LINUX.md

N'hésitez pas à jeter un oeil à la documentation sur le site officiel de Sonic Pi pour vous familiariser avec l'environnement.

## Mélodie d'introduction

```
# Paramétrage
use_debug false # help RPi performance
use_bpm 100
use_synth :pulse
use_synth_defaults release: 0.2, mod_rate: 5, amp: 0.6

## Mélodie d'introduction
## les notes sont représentées par la notation anglosaxonne.
## "nil" représente une pause
play_pattern_timed([:e5,:e5,nil,:e5,nil,:c5,:e5,nil,
                    :g5,nil,nil,nil,nil,nil,nil,nil], [0.25])
```

## Et la suite de la mélodie

### Thème A

Ajouter le code suivant à la suite de la première mélodie
```
# Thème A
play_pattern_timed([:c5,nil,nil,:g4,nil,nil,:e4,nil,
                    nil,:a4,nil,:b4,nil,:Bb4,:a4,nil], [0.25])
play_pattern_timed([:g4,:e5,:g5], [1/3.0]) # minim triplets
play_pattern_timed([:a5,nil,:f5,:g5,
                    nil,:e5,nil,:c5,
                    :d5,:b4,nil,nil], [0.25])
```


### Thème B

Ajouter le code suivant à la suite de la première mélodie
```
# Thème B
play_pattern_timed([nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                    nil,:gs4,:a4,:c5,nil,:a4,:c5,:d5,
                    nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                    nil,:c6,nil,:c6,:c6,nil,nil,nil,
                    nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                    nil,:gs4,:a4,:c5,nil,:a4,:c5,:d5,
                    nil,nil,:ds5,nil,nil,:d5,nil,nil,
                    :c5,nil,nil,nil,nil,nil,nil,nil], [0.25])
```


### Thème C

Ajouter le code suivant à la suite de la première mélodie
```
# Thème C
play_pattern_timed([:c5,:c5,nil,:c5,nil,:c5,:d5,nil,
                    :e5,:c5,nil,:a4,:g4,nil,nil,nil,
                    :c5,:c5,nil,:c5,nil,:c5,:d5,:e5,
                    nil,nil,nil,nil,nil,nil,nil,nil,
                    :c5,:c5,nil,:c5,nil,:c5,:d5,nil,
                    :e5,:c5,nil,:a4,:g4,nil,nil,nil,
                    :e5,:e5,nil,:e5,nil,:c5,:e5,nil,
                    :g5,nil,nil,nil,nil,nil,nil,nil], [0.25])
```

### Thème D

Ajouter le code suivant à la suite de la première mélodie
```
# Thème D
play_pattern_timed([:e5,:c5,nil,:g4,nil,nil,:gs4,nil,
                    :a4,:f5,nil,:f5,:a4,nil,nil,nil], [0.25])
play_pattern_timed([:b4,:a5,:a5,
                    :a5,:g5,:f5], [1/3.0])
play_pattern_timed([:e5,:c5,nil,:a4,:g4,nil,nil,nil], [0.25])
play_pattern_timed([:e5,:c5,nil,:g4,nil,nil,:gs4,nil,
                    :a4,:f5,nil,:f5,:a4,nil,nil,nil,
                    :b4,:f5,nil,:f5], [0.25])
play_pattern_timed([:f5,:e5,:d5], [1/3.0])
play_pattern_timed([:g5,:e5,nil,:e5,:c5,nil,nil,nil], [0.25])
```

Chouette, on a tous les bouts de la mélodie ! Par contre, certaines portions doivent être présentes plusieurs fois.
Pour cela, nous allons regrouper les différentes parties du thème dans des fonctions que nous pourrons appeler plusieurs fois.

## Factorisation

On définit une méthode en la définissant telle que :
```nomDeLaFonction = -> {
    # ce qu'on veut mettre à l'intérieur
}
```

Nous allons donc créer 5 fonctions, une pour chacune des portions de la mélodie principale :

```
use_debug false # help RPi performance
use_bpm 100
use_synth :pulse
use_synth_defaults release: 0.2, mod_rate: 5, amp: 0.6

# Intro
intro = -> {play_pattern_timed([:e5,:e5,nil,:e5,nil,:c5,:e5,nil,
                                :g5,nil,nil,nil,nil,nil,nil,nil], [0.25])}

# Thème A
theme_a = -> {
  play_pattern_timed([:c5,nil,nil,:g4,nil,nil,:e4,nil,
                      nil,:a4,nil,:b4,nil,:Bb4,:a4,nil], [0.25])
  play_pattern_timed([:g4,:e5,:g5], [1/3.0]) # minim triplets
  play_pattern_timed([:a5,nil,:f5,:g5,
                      nil,:e5,nil,:c5,
                      :d5,:b4,nil,nil], [0.25])
}

# Thème B
theme_b = -> {
  play_pattern_timed([nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                      nil,:gs4,:a4,:c5,nil,:a4,:c5,:d5,
                      nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                      nil,:c6,nil,:c6,:c6,nil,nil,nil,
                      nil,nil,:g5,:fs5,:f5,:ds5,nil,:e5,
                      nil,:gs4,:a4,:c5,nil,:a4,:c5,:d5,
                      nil,nil,:ds5,nil,nil,:d5,nil,nil,
                      :c5,nil,nil,nil,nil,nil,nil,nil], [0.25])
}

# Thème C
theme_c = -> {
  play_pattern_timed([:c5,:c5,nil,:c5,nil,:c5,:d5,nil,
                      :e5,:c5,nil,:a4,:g4,nil,nil,nil,
                      :c5,:c5,nil,:c5,nil,:c5,:d5,:e5,
                      nil,nil,nil,nil,nil,nil,nil,nil,
                      :c5,:c5,nil,:c5,nil,:c5,:d5,nil,
                      :e5,:c5,nil,:a4,:g4,nil,nil,nil,
                      :e5,:e5,nil,:e5,nil,:c5,:e5,nil,
                      :g5,nil,nil,nil,nil,nil,nil,nil], [0.25])
}

# Thème D
theme_d = -> {
  play_pattern_timed([:e5,:c5,nil,:g4,nil,nil,:gs4,nil,
                      :a4,:f5,nil,:f5,:a4,nil,nil,nil], [0.25])
  play_pattern_timed([:b4,:a5,:a5,
                      :a5,:g5,:f5], [1/3.0])
  play_pattern_timed([:e5,:c5,nil,:a4,:g4,nil,nil,nil], [0.25])
  play_pattern_timed([:e5,:c5,nil,:g4,nil,nil,:gs4,nil,
                      :a4,:f5,nil,:f5,:a4,nil,nil,nil,
                      :b4,:f5,nil,:f5], [0.25])
  play_pattern_timed([:f5,:e5,:d5], [1/3.0])
  play_pattern_timed([:g5,:e5,nil,:e5,:c5,nil,nil,nil], [0.25])
}

```

Si on joue notre code tel quel, rien ne se passe. Nous n'avons que définit les fonctions sans les appeler.
Nous allons donc les appeler, une ou plusieurs fois chacune pour obtenir la mélodie finale.
Ajouter les appels suivants à la fin du code :

```
1.times { intro.call }
2.times { theme_a.call }
2.times { theme_b.call }
1.times { theme_c.call }
2.times { theme_a.call }
2.times { theme_d.call }
1.times { theme_c.call }
1.times { theme_d.call }
```

On a mis des "étiquettes" aux différents thèmes de la mélodie.
On utilise `x.times` pour répéter une portion de code plusieurs fois.
Au final, on obtient la musique complète avec les portions de mélodie répétées.

On a la mélodie principale, mais elle semble un peu "nue" : il nous manque les accompagnements, les percussions, ...
Voyons comment ajouter ces éléments à notre mélodie.

## Buffers
Il est possible de tester différentes choses dans des contextes indépendants. Pour cela, vous pouvez utiliser les buffers disponibles en bas de l'écran.
Dans un nouveau buffer, copiez la mélodie suivante (premier accompagnement)
```
in_thread do
  intro = -> { play_pattern_timed([:fs4,:fs4,nil,:fs4,nil,:fs4,:fs4,nil,
                                   :b4,nil,nil,nil,:g4,nil,nil,nil], [0.25]) }
  theme_a = -> {
    play_pattern_timed([:e4,nil,nil,:c4,nil,nil,:g3,nil,
                        nil,:c4,nil,:d4,nil,:Db4,:c4,nil], [0.25])
    play_pattern_timed([:c4,:g4,:b4], [1/3.0])
    play_pattern_timed([:c5,nil,:a4,:b4,
                        nil,:a4,nil,:e4,
                        :f4,:d4,nil,nil], [0.25]) }
  theme_b = -> {
    play_pattern_timed([nil,nil,:e5,:ds5,:d5,:b4,nil,:c5,
                        nil,:e4,:f4,:g4,nil,:c4,:e4,:f4,
                        nil,nil,:e5,:ds5,:d5,:b4,nil,:c5,
                        nil,:f5,nil,:f5,:f5,nil,nil,nil,
                        nil,nil,:e5,:ds5,:d5,:b4,nil,:c5,
                        nil,:e4,:f4,:g4,nil,:c4,:e4,:f4,
                        nil,nil,:gs4,nil,nil,:f4,nil,nil,
                        :e4,nil,nil,nil,nil,nil,nil,nil], [0.25]) }
  theme_c = -> {
    play_pattern_timed([:gs4,:gs4,nil,:gs4,nil,:gs4,:as4,nil,
                        :g4,:e4,nil,:e4,:c4,nil,nil,nil,
                        :gs4,:gs4,nil,:gs4,nil,:gs4,:as4,:g4,
                        nil,nil,nil,nil,nil,nil,nil,nil,
                        :gs4,:gs4,nil,:gs4,nil,:gs4,:as4,nil,
                        :g4,:e4,nil,:e4,:c4,nil,nil,nil,
                        :fs4,:fs4,nil,:fs4,nil,:fs4,:fs4,nil,
                        :b4,nil,nil,nil,:g4,nil,nil,nil], [0.25]) }
  theme_d = -> {
    play_pattern_timed([:c5,:a4,nil,:e4,nil,nil,:e4,nil,
                        :f4,:c5,nil,:c5,:f4,nil,nil,nil], [0.25])
    play_pattern_timed([:g4,:f5,:f5,
                        :f5,:e5,:d5], [1/3.0])
    play_pattern_timed([:c5,:a4,nil,:f4,:e4,nil,nil,nil], [0.25])
    play_pattern_timed([:c5,:a4,nil,:e4,nil,nil,:e4,nil,
                        :f4,:c5,nil,:c5,:f4,nil,nil,nil,
                        :g4,:d5,nil,:d5], [0.25])
    play_pattern_timed([:d5,:c5,:b4], [1/3.0])
  play_pattern_timed([:c5,nil,nil,nil,nil,nil,nil,nil], [0.25]) }

  1.times { intro.call }
  2.times { theme_a.call }
  2.times { theme_b.call }
  1.times { theme_c.call }
  2.times { theme_a.call }
  2.times { theme_d.call }
  1.times { theme_c.call }
  1.times { theme_d.call }
end
```
Jouée seule, cette mélodie semble un peu étrange. Il faudrait réussir à la jouer en même temps que la première !

## Thread

Sonic Pi offre la possibilité de jouer plusieurs mélodies en parallèle. On parle alors de threads.
Commençons déjà par inclure notre mélodie principale dans un thread dédié en utilisant :
```
in_thread do
  # Ce qu'on veut faire dans note fil
end
```

Imbriquez votre premier code à l'intérieur d'un nouveau thread.

```
use_debug false # help RPi performance
use_bpm 100
use_synth :pulse
use_synth_defaults release: 0.2, mod_rate: 5, amp: 0.6

in_thread do
  # ...
end
```

Aucun changement lorsque l'on lance le programme. La mélodie est la même.
Mais nous allons maintenant pouvoir ajouter la première mélodie d'accompagnement en parallèle.
Copiez le code de l'accompagnement dans un nouveau thread et ajoutez-le à la suite du précédent.

On commence à bien reproduire le son "chip tune" caractéristique des anciennes consoles.

Ajoutons encore un 3ème thread :
```
in_thread do
  use_synth :tri
  use_synth_defaults attack: 0, sustain: 0.1, decay: 0.1, release: 0.1, amp: 0.4

  intro = -> { play_pattern_timed([:D4,:D4,nil,:D4,nil,:D4,:D4,nil,
                                   :G3,nil,nil,nil,:G4,nil,nil,nil], [0.25]) }
  theme_a = -> {
    play_pattern_timed([:G4,nil,nil,:E4,nil,nil,:C4,nil,
                        nil,:F4,nil,:G4,nil,:Gb4,:F4,nil], [0.25])
    play_pattern_timed([:E4,:C4,:E4], [1/3.0])
    play_pattern_timed([:F4,nil,:D4,:E4,
                        nil,:C4,nil,:A3,
                        :B3,:G3,nil,nil], [0.25]) }
  theme_b = -> {
    play_pattern_timed([:C3,nil,nil,:G3,nil,nil,:C3,nil,
                        :F3,nil,nil,:C3,:C3,nil,:F3,nil,
                        :C3,nil,nil,:E3,nil,nil,:G3,:C3,
                        nil,:G2,nil,:G2,:G2,nil,:G4,nil,
                        :C3,nil,nil,:G3,nil,nil,:C3,nil,
                        :F3,nil,nil,:C3,:C3,nil,:F3,nil,
                        :C3,nil,:Ab3,nil,nil,:Bb3,nil,nil,
                        :C3,nil,nil,:G2,:G2,nil,:C3,nil], [0.25]) }
  theme_c = -> {
    3.times {
      play_pattern_timed([:gs4,nil,nil,:ds4,nil,nil,:gs4,nil,
                          :g4,nil,nil,:c4,nil,nil,:g4,nil], [0.25])
    }
    play_pattern_timed([:D4,:D4,nil,:D4,nil,:D4,:D4,nil,
                        :G3,nil,nil,nil,:G4,nil,nil,nil], [0.25]) }
  theme_d = -> {
    play_pattern_timed([:C3,nil,nil,:fs3,:g3,nil,:C3,nil,
                        :F3,nil,:F3,nil,:C3,:C3,:F3,nil,
                        :D3,nil,nil,:F3,:G3,nil,:B3,nil,
                        :G3,nil,:G3,nil,:C3,:C3,:G3,nil,
                        :C3,nil,nil,:fs3,:g3,nil,:C3,nil,
                        :F3,nil,:F3,nil,:C3,:C3,:F3,nil,
                        :G3,nil,nil,:G3], [0.25])
    play_pattern_timed([:G3,:A3,:B3], [1/3.0])
  play_pattern_timed([:C4,nil,:G3,nil,:C4,nil,nil,nil], [0.25]) }

  1.times { intro.call }
  2.times { theme_a.call }
  2.times { theme_b.call }
  1.times { theme_c.call }
  2.times { theme_a.call }
  2.times { theme_d.call }
  1.times { theme_c.call }
  1.times { theme_d.call }
end
```

On y est presque ! Il ne manque que les percussions

## Percussions

Les "percussions" vont être gérées de manière un peu différentes.
Ici, on ne reprend pas les grands thèmes de la mélodie. On va utiliser un seul et même modèle pour toute la chanson.
N'hésitez pas à le tester unitairement en le copiant dans un nouveau buffer (onglet disponible en bas de l'écran)

```
in_thread do
  use_synth :fm
  use_synth_defaults divisor: 1.6666, attack: 0.0, depth: 1500, sustain: 0.05, release: 0.0

  ll = -> { play :a, sustain: 0.1; sleep 0.75 }
  l = -> { play :a, sustain: 0.1; sleep 0.5 }
  s = -> { play :a; sleep 0.25 }

  define :drum_pattern_a do
    [l,s,l,s,l,ll,l,s,s,s].map(&:call)
  end

  define :drum_pattern_b do
    play :b
    sleep 0.5
    play :a6
    sleep 0.3
    play :a7
    sleep 0.2
    play :a, sustain: 0.1
    sleep 0.5
    play :a6
    sleep 0.3
    play :a7
    sleep 0.2
  end

  define :drum_pattern_c do
    [ll,s,l,l].map(&:call)
  end

  with_fx :level, amp: 0.5 do
    1.times  { drum_pattern_a }
    loop do
      24.times { drum_pattern_b }
      4.times  { drum_pattern_a }
      8.times  { drum_pattern_b }
      16.times { drum_pattern_c }
      4.times  { drum_pattern_a }
      8.times  { drum_pattern_b }
    end
  end
end
```
Et voilà ! Nous avons réussit à reproduire le thème de Mario :-)
