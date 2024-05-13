<script setup>
// this app will show a form to user
// user will enter which bridge board number they played
// user will enter NS and EW team numbers
// user will enter the contract
// user will enter the declarer
// user will enter the result
// app will calculate the score
// app will show the score

import { ref, computed, reactive } from 'vue'

const board = ref('')
const nsTeam = ref('')
const ewTeam = ref('')
const contract1 = ref('')
const contract2 = ref('')
const contractorSide = ref('')
const double = ref(false)
const redouble = ref(false)
const result = ref('')


const positiveScoreList = {
  'NON_VULNERABLE': {
    'MAJOR': {
      '1': {
        'BIEN': {
          'PASS': 80,
          'X': 160,
          'XX': 520,
        },
        '1': {
          'PASS': 110,
          'X': 260,
          'XX': 720,
        },
        '2': {
          'PASS': 140,
          'X': 360,
          'XX': 920,
        },
        '3': {
          'PASS': 170,
          'X': 460,
          'XX': 1120,
        },
        '4': {
          'PASS': 200,
          'X': 560,
          'XX': 1320,
        },
        '5': {
          'PASS': 230,
          'X': 660,
          'XX': 1520,
        },
        '6': {
          'PASS': 260,
          'X': 760,
          'XX': 1720,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 110,
          'X': 470,
          'XX': 640,
        },
        '1': {
          'PASS': 140,
          'X': 570,
          'XX': 840,
        },
        '2': {
          'PASS': 170,
          'X': 670,
          'XX': 1040,
        },
        '3': {
          'PASS': 200,
          'X': 770,
          'XX': 1240,
        },
        '4': {
          'PASS': 230,
          'X': 870,
          'XX': 1440,
        },
        '5': {
          'PASS': 260,
          'X': 970,
          'XX': 1640,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 140,
          'X': 530,
          'XX': 760,
        },
        '1': {
          'PASS': 170,
          'X': 630,
          'XX': 960,
        },
        '2': {
          'PASS': 200,
          'X': 730,
          'XX': 1160,
        },
        '3': {
          'PASS': 230,
          'X': 830,
          'XX': 1360,
        },
        '4': {
          'PASS': 260,
          'X': 930,
          'XX': 1560,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 420,
          'X': 590,
          'XX': 880,
        },
        '1': {
          'PASS': 450,
          'X': 690,
          'XX': 1080,
        },
        '2': {
          'PASS': 480,
          'X': 790,
          'XX': 1280,
        },
        '3': {
          'PASS': 510,
          'X': 890,
          'XX': 1480,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 450,
          'X': 650,
          'XX': 1000,
        },
        '1': {
          'PASS': 480,
          'X': 750,
          'XX': 1200,
        },
        '2': {
          'PASS': 510,
          'X': 850,
          'XX': 1400,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 980,
          'X': 1210,
          'XX': 1620,
        },
        '1': {
          'PASS': 1010,
          'X': 1310,
          'XX': 1820,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 1510,
          'X': 1770,
          'XX': 2240,
        }
      }
    },
    'MINOR': {
      '1': {
        'BIEN': {
          'PASS': 70,
          'X': 140,
          'XX': 230,
        },
        '1': {
          'PASS': 90,
          'X': 240,
          'XX': 430,
        },
        '2': {
          'PASS': 110,
          'X': 340,
          'XX': 630,
        },
        '3': {
          'PASS': 130,
          'X': 440,
          'XX': 830,
        },
        '4': {
          'PASS': 150,
          'X': 540,
          'XX': 1030,
        },
        '5': {
          'PASS': 170,
          'X': 640,
          'XX': 1230,
        },
        '6': {
          'PASS': 190,
          'X': 740,
          'XX': 1430,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 90,
          'X': 180,
          'XX': 560,
        },
        '1': {
          'PASS': 110,
          'X': 280,
          'XX': 760,
        },
        '2': {
          'PASS': 130,
          'X': 380,
          'XX': 960,
        },
        '3': {
          'PASS': 150,
          'X': 480,
          'XX': 1160,
        },
        '4': {
          'PASS': 170,
          'X': 580,
          'XX': 1360,
        },
        '5': {
          'PASS': 190,
          'X': 680,
          'XX': 1560,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 110,
          'X': 470,
          'XX': 640,
        },
        '1': {
          'PASS': 130,
          'X': 570,
          'XX': 840,
        },
        '2': {
          'PASS': 150,
          'X': 670,
          'XX': 1040,
        },
        '3': {
          'PASS': 170,
          'X': 770,
          'XX': 1240,
        },
        '4': {
          'PASS': 190,
          'X': 870,
          'XX': 1440,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 130,
          'X': 510,
          'XX': 720,
        },
        '1': {
          'PASS': 150,
          'X': 610,
          'XX': 920,
        },
        '2': {
          'PASS': 170,
          'X': 710,
          'XX': 1120,
        },
        '3': {
          'PASS': 190,
          'X': 810,
          'XX': 1320,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 400,
          'X': 550,
          'XX': 800,
        },
        '1': {
          'PASS': 420,
          'X': 650,
          'XX': 1000,
        },
        '2': {
          'PASS': 440,
          'X': 750,
          'XX': 1200,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 920,
          'X': 1090,
          'XX': 1380,
        },
        '1': {
          'PASS': 940,
          'X': 1190,
          'XX': 1580,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 1440,
          'X': 1630,
          'XX': 1960,
        }
      }
    },
    'NT': {
      '1': {
        'BIEN': {
          'PASS': 90,
          'X': 180,
          'XX': 560,
        },
        '1': {
          'PASS': 120,
          'X': 280,
          'XX': 760,
        },
        '2': {
          'PASS': 150,
          'X': 380,
          'XX': 960,
        },
        '3': {
          'PASS': 180,
          'X': 480,
          'XX': 1160,
        },
        '4': {
          'PASS': 210,
          'X': 580,
          'XX': 1360,
        },
        '5': {
          'PASS': 240,
          'X': 680,
          'XX': 1560,
        },
        '6': {
          'PASS': 270,
          'X': 780,
          'XX': 1760,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 120,
          'X': 490,
          'XX': 680,
        },
        '1': {
          'PASS': 150,
          'X': 590,
          'XX': 880,
        },
        '2': {
          'PASS': 180,
          'X': 690,
          'XX': 1080,
        },
        '3': {
          'PASS': 210,
          'X': 790,
          'XX': 1280,
        },
        '4': {
          'PASS': 240,
          'X': 890,
          'XX': 1480,
        },
        '5': {
          'PASS': 270,
          'X': 990,
          'XX': 1680,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 400,
          'X': 550,
          'XX': 800,
        },
        '1': {
          'PASS': 430,
          'X': 650,
          'XX': 1000,
        },
        '2': {
          'PASS': 460,
          'X': 750,
          'XX': 1200,
        },
        '3': {
          'PASS': 490,
          'X': 850,
          'XX': 1400,
        },
        '4': {
          'PASS': 520,
          'X': 950,
          'XX': 1600,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 430,
          'X': 610,
          'XX': 920,
        },
        '1': {
          'PASS': 460,
          'X': 710,
          'XX': 1120,
        },
        '2': {
          'PASS': 490,
          'X': 810,
          'XX': 1320,
        },
        '3': {
          'PASS': 520,
          'X': 910,
          'XX': 1520,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 460,
          'X': 670,
          'XX': 1040,
        },
        '1': {
          'PASS': 490,
          'X': 770,
          'XX': 1240,
        },
        '2': {
          'PASS': 520,
          'X': 870,
          'XX': 1440,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 990,
          'X': 1230,
          'XX': 1660,
        },
        '1': {
          'PASS': 1020,
          'X': 1330,
          'XX': 1860,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 1520,
          'X': 1790,
          'XX': 2280,
        }
      }
    },
  },
  'VULNERABLE': {
    'MAJOR': {
      '1': {
        'BIEN': {
          'PASS': 80,
          'X': 160,
          'XX': 720,
        },
        '1': {
          'PASS': 110,
          'X': 360,
          'XX': 1120,
        },
        '2': {
          'PASS': 140,
          'X': 560,
          'XX': 1520,
        },
        '3': {
          'PASS': 170,
          'X': 760,
          'XX': 1920,
        },
        '4': {
          'PASS': 200,
          'X': 960,
          'XX': 2320,
        },
        '5': {
          'PASS': 230,
          'X': 1160,
          'XX': 2720,
        },
        '6': {
          'PASS': 260,
          'X': 1360,
          'XX': 3120,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 110,
          'X': 670,
          'XX': 840,
        },
        '1': {
          'PASS': 140,
          'X': 870,
          'XX': 1240,
        },
        '2': {
          'PASS': 170,
          'X': 1070,
          'XX': 1640,
        },
        '3': {
          'PASS': 200,
          'X': 1270,
          'XX': 2040,
        },
        '4': {
          'PASS': 230,
          'X': 1470,
          'XX': 2440,
        },
        '5': {
          'PASS': 260,
          'X': 1670,
          'XX': 2840,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 140,
          'X': 730,
          'XX': 960,
        },
        '1': {
          'PASS': 170,
          'X': 930,
          'XX': 1360,
        },
        '2': {
          'PASS': 200,
          'X': 1130,
          'XX': 1760,
        },
        '3': {
          'PASS': 230,
          'X': 1330,
          'XX': 2160,
        },
        '4': {
          'PASS': 260,
          'X': 1530,
          'XX': 2560,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 620,
          'X': 790,
          'XX': 1080,
        },
        '1': {
          'PASS': 650,
          'X': 990,
          'XX': 1480,
        },
        '2': {
          'PASS': 680,
          'X': 1190,
          'XX': 1880,
        },
        '3': {
          'PASS': 710,
          'X': 1390,
          'XX': 2280,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 650,
          'X': 850,
          'XX': 1200,
        },
        '1': {
          'PASS': 680,
          'X': 1050,
          'XX': 1600,
        },
        '2': {
          'PASS': 710,
          'X': 1250,
          'XX': 2000,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 1430,
          'X': 1660,
          'XX': 2070,
        },
        '1': {
          'PASS': 1460,
          'X': 1860,
          'XX': 2470,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 2210,
          'X': 2470,
          'XX': 2940,
        }
      }
    },
    'MINOR': {
      '1': {
        'BIEN': {
          'PASS': 70,
          'X': 140,
          'XX': 230,
        },
        '1': {
          'PASS': 90,
          'X': 340,
          'XX': 630,
        },
        '2': {
          'PASS': 110,
          'X': 540,
          'XX': 1030,
        },
        '3': {
          'PASS': 130,
          'X': 740,
          'XX': 1430,
        },
        '4': {
          'PASS': 150,
          'X': 940,
          'XX': 1830,
        },
        '5': {
          'PASS': 170,
          'X': 1040,
          'XX': 2230,
        },
        '6': {
          'PASS': 190,
          'X': 1340,
          'XX': 2630,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 90,
          'X': 180,
          'XX': 760,
        },
        '1': {
          'PASS': 110,
          'X': 380,
          'XX': 1160,
        },
        '2': {
          'PASS': 130,
          'X': 580,
          'XX': 1560,
        },
        '3': {
          'PASS': 150,
          'X': 780,
          'XX': 1960,
        },
        '4': {
          'PASS': 170,
          'X': 980,
          'XX': 2360,
        },
        '5': {
          'PASS': 190,
          'X': 1180,
          'XX': 2760,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 110,
          'X': 670,
          'XX': 840,
        },
        '1': {
          'PASS': 130,
          'X': 870,
          'XX': 1240,
        },
        '2': {
          'PASS': 150,
          'X': 1070,
          'XX': 1640,
        },
        '3': {
          'PASS': 170,
          'X': 1270,
          'XX': 2040,
        },
        '4': {
          'PASS': 190,
          'X': 1470,
          'XX': 2440,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 130,
          'X': 710,
          'XX': 920,
        },
        '1': {
          'PASS': 150,
          'X': 910,
          'XX': 1320,
        },
        '2': {
          'PASS': 170,
          'X': 1110,
          'XX': 1720,
        },
        '3': {
          'PASS': 190,
          'X': 1310,
          'XX': 2120,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 600,
          'X': 750,
          'XX': 1000,
        },
        '1': {
          'PASS': 620,
          'X': 950,
          'XX': 1400,
        },
        '2': {
          'PASS': 640,
          'X': 1150,
          'XX': 1800,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 1370,
          'X': 1540,
          'XX': 1830,
        },
        '1': {
          'PASS': 1390,
          'X': 1740,
          'XX': 2230,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 2140,
          'X': 2230,
          'XX': 2660,
        }
      }
    },
    'NT': {
      '1': {
        'BIEN': {
          'PASS': 90,
          'X': 180,
          'XX': 760,
        },
        '1': {
          'PASS': 120,
          'X': 380,
          'XX': 1160,
        },
        '2': {
          'PASS': 150,
          'X': 580,
          'XX': 1560,
        },
        '3': {
          'PASS': 180,
          'X': 780,
          'XX': 1960,
        },
        '4': {
          'PASS': 210,
          'X': 980,
          'XX': 2360,
        },
        '5': {
          'PASS': 240,
          'X': 1180,
          'XX': 2760,
        },
        '6': {
          'PASS': 270,
          'X': 1380,
          'XX': 3160,
        },
      },
      '2': {
        'BIEN': {
          'PASS': 120,
          'X': 690,
          'XX': 880,
        },
        '1': {
          'PASS': 150,
          'X': 890,
          'XX': 1280,
        },
        '2': {
          'PASS': 180,
          'X': 1090,
          'XX': 1680,
        },
        '3': {
          'PASS': 210,
          'X': 1290,
          'XX': 2080,
        },
        '4': {
          'PASS': 240,
          'X': 1490,
          'XX': 2480,
        },
        '5': {
          'PASS': 270,
          'X': 1690,
          'XX': 2880,
        }
      },
      '3': {
        'BIEN': {
          'PASS': 600,
          'X': 750,
          'XX': 1000,
        },
        '1': {
          'PASS': 630,
          'X': 950,
          'XX': 1400,
        },
        '2': {
          'PASS': 660,
          'X': 1550,
          'XX': 1800,
        },
        '3': {
          'PASS': 690,
          'X': 1350,
          'XX': 2200,
        },
        '4': {
          'PASS': 720,
          'X': 1550,
          'XX': 2600,
        }
      },
      '4': {
        'BIEN': {
          'PASS': 630,
          'X': 810,
          'XX': 1120,
        },
        '1': {
          'PASS': 660,
          'X': 1010,
          'XX': 1520,
        },
        '2': {
          'PASS': 690,
          'X': 1210,
          'XX': 1920,
        },
        '3': {
          'PASS': 720,
          'X': 1410,
          'XX': 2320,
        }
      },
      '5': {
        'BIEN': {
          'PASS': 660,
          'X': 870,
          'XX': 1240,
        },
        '1': {
          'PASS': 890,
          'X': 1070,
          'XX': 1640,
        },
        '2': {
          'PASS': 720,
          'X': 1270,
          'XX': 2040,
        }
      },
      '6': {
        'BIEN': {
          'PASS': 1440,
          'X': 1680,
          'XX': 2110,
        },
        '1': {
          'PASS': 1470,
          'X': 1880,
          'XX': 2510,
        }
      },
      '7': {
        'BIEN': {
          'PASS': 2220,
          'X': 2490,
          'XX': 2980,
        }
      }
    },
  },
}

const negativeScoreList = {
  'NON_VULNERABLE': {
    'PASS': {
      '-1': 50,
      '-2': 100,
      '-3': 150,
      '-4': 200,
      '-5': 250,
      '-6': 300,
      '-7': 350,
      '-8': 400,
      '-9': 450,
      '-10': 500,
      '-11': 550,
      '-12': 600,
      '-13': 650,
    },
    'X': {
      '-1': 100,
      '-2': 300,
      '-3': 500,
      '-4': 800,
      '-5': 1100,
      '-6': 1400,
      '-7': 1700,
      '-8': 2000,
      '-9': 2300,
      '-10': 2600,
      '-11': 2900,
      '-12': 3200,
      '-13': 3500,
    },
    'XX': {
      '-1': 200,
      '-2': 600,
      '-3': 1000,
      '-4': 1600,
      '-5': 2200,
      '-6': 2800,
      '-7': 3400,
      '-8': 4000,
      '-9': 4600,
      '-10': 5200,
      '-11': 5800,
      '-12': 6400,
      '-13': 7000,
    },
  },
  'VULNERABLE': {
    'PASS': {
      '-1': 100,
      '-2': 200,
      '-3': 300,
      '-4': 400,
      '-5': 500,
      '-6': 600,
      '-7': 700,
      '-8': 800,
      '-9': 900,
      '-10': 1000,
      '-11': 1100,
      '-12': 1200,
      '-13': 1300,
    },
    'X': {
      '-1': 200,
      '-2': 500,
      '-3': 800,
      '-4': 1100,
      '-5': 1400,
      '-6': 1700,
      '-7': 2000,
      '-8': 2300,
      '-9': 2600,
      '-10': 2900,
      '-11': 3200,
      '-12': 3500,
      '-13': 3800,
    },
    'XX': {
      '-1': 400,
      '-2': 1000,
      '-3': 1600,
      '-4': 2200,
      '-5': 2800,
      '-6': 3400,
      '-7': 4000,
      '-8': 4600,
      '-9': 5200,
      '-10': 5800,
      '-11': 6400,
      '-12': 7000,
      '-13': 7600,
    },
  },
}

const boardValnurability = {
  '1': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '2': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '3': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '4': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '5': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '6': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '7': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '8': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '9': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '10': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '11': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '12': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '13': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '14': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '15': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '16': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '17': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '18': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '19': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '20': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '21': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '22': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '23': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '24': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '25': {
    'NS': 'NON_VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '26': {
    'NS': 'VULNERABLE',
    'EW': 'VULNERABLE',
  },
  '27': {
    'NS': 'NON_VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
  '28': {
    'NS': 'VULNERABLE',
    'EW': 'NON_VULNERABLE',
  },
}

// computed properties
const NS_vulnarability = computed(() => {
  if (boardValnurability[board.value])
    return boardValnurability[board.value].NS;

  return null;
})

const EW_vulnarability = computed(() => {
  if (boardValnurability[board.value])
    return boardValnurability[board.value].EW;

  return null;
})

const resultVulnarability = computed(() => {
  if (boardValnurability[board.value] && contractorSide.value)
    return boardValnurability[board.value][contractorSide.value];
})

const level = computed(() => {
  if (contract2.value === "NT")
    return "NT";

  if (contract2.value === "SPADES" || contract2.value === "HEARTS")
    return "MAJOR";

  if (contract2.value === "DIAMONDS" || contract2.value === "CLUBS")
    return "MINOR";

  return null;
})

const resultNegativeOrPositive = computed(() => {
  if (result.value < 0)
    return "NEGATIVE";

  return "POSITIVE";
})

const contractType = computed(() => {
  if (double.value && redouble.value)
    return "XX";

  if (double.value && !redouble.value)
    return "X";

  return "PASS";
})

const sideToAddPoints = computed(() => {
  if (contractorSide.value === "NS") {
    if (resultNegativeOrPositive.value === "POSITIVE")
      return "NS";

    if (resultNegativeOrPositive.value === "NEGATIVE")
      return "EW";
  }

  if (contractorSide.value === "EW") {
    if (resultNegativeOrPositive.value === "POSITIVE")
      return "EW";

    if (resultNegativeOrPositive.value === "NEGATIVE")
      return "NS";
  }

  return null;
})

const score = computed(() => {
  if (resultNegativeOrPositive.value === "NEGATIVE" && resultVulnarability.value && contractType.value && result.value) {
    return negativeScoreList[resultVulnarability.value][contractType.value][result.value];
  }

  if (resultNegativeOrPositive.value === "POSITIVE" && level.value && contract1.value && contractType.value && result.value) {
    return positiveScoreList[resultVulnarability.value][level.value][contract1.value][result.value][contractType.value];
  }

  return null;
})


</script>

<template>
  <main>
    Board: {{ board }}
    <br>
    NS Vulnerability: {{ NS_vulnarability }}
    <br>
    EW Vulnerability: {{ EW_vulnarability }}
    <br>
    Contractor Side: {{ contractorSide }}
    <br>
    Result Vulnerability: {{ resultVulnarability }}
    <br>
    Level: {{ level }}
    <br>
    Result: {{ result }}
    <br>
    Result Type: {{ resultNegativeOrPositive }}
    <br>
    Contract Type: {{ contractType }}
    <br>
    Side To Add Points: {{ sideToAddPoints }}
    <br>
    Score: {{ score }}
    <hr>
    <form action="" method="post">
      <label for="board">Board Numarası</label>
      <input type="text" name="board" id="board" v-model="board">
      <hr>
      <label for="nsTeam">NS</label>
      <input type="text" name="nsTeam" id="nsTeam" v-model="nsTeam">
      <hr>
      <label for="ewTeam">EW</label>
      <input type="text" name="ewTeam" id="ewTeam" v-model="ewTeam">
      <hr>
      <label for="contract">Kontrat</label>
      <br>
      <!-- button group -->
      <input type="radio" name="contract1" value="1" id="1" v-model="contract1"><label for="1">1</label>
      <input type="radio" name="contract1" value="2" id="2" v-model="contract1"><label for="2">2</label>
      <input type="radio" name="contract1" value="3" id="3" v-model="contract1"><label for="3">3</label>
      <input type="radio" name="contract1" value="4" id="4" v-model="contract1"><label for="4">4</label>
      <input type="radio" name="contract1" value="5" id="5" v-model="contract1"><label for="5">5</label>
      <input type="radio" name="contract1" value="6" id="6" v-model="contract1"><label for="6">6</label>
      <input type="radio" name="contract1" value="7" id="7" v-model="contract1"><label for="7">7</label>
      <!-- button group -->
      <br>
      <input type="radio" name="contract2" value="SPADES" id="♠️" v-model="contract2"><label for="♠️">♠️</label>
      <input type="radio" name="contract2" value="HEARTS" id="❤️" v-model="contract2"><label for="❤️">❤️</label>
      <input type="radio" name="contract2" value="DIAMONDS" id="♦️" v-model="contract2"><label for="♦️">♦️</label>
      <input type="radio" name="contract2" value="CLUBS" id="♣️" v-model="contract2"><label for="♣️">♣️</label>
      <input type="radio" name="contract2" value="NT" id="NT" v-model="contract2"><label for="NT">NT</label>
      <hr>
      <label for="contractorSide">Kontratı Oynayan Taraf</label>
      <input type="radio" name="contractorSide" value="NS" id="NS" v-model="contractorSide"><label for="NS">NS</label>
      <input type="radio" name="contractorSide" value="EW" id="EW" v-model="contractorSide"><label for="EW">EW</label>
      <hr>
      <label for="double">Kontr</label>
      <input type="checkbox" name="double" id="double" v-model="double">
      <hr>
      <label for="redouble">Sürkontr</label>
      <input type="checkbox" name="redouble" id="redouble" v-model="redouble">
      <hr>
      <label for="result">Sonuç</label>
      <br>

      <template v-if="contract1 >= 7">
        <input type="radio" name="result" value="-13" id="result_13" v-model="result"><label for="result_13">-13</label>
      </template>
      <template v-if="contract1 >= 6">
        <input type="radio" name="result" value="-12" id="result_12" v-model="result"><label for="result_12">-12</label>
      </template>
      <template v-if="contract1 >= 5">
        <input type="radio" name="result" value="-11" id="result_11" v-model="result"><label for="result_11">-11</label>
      </template>
      <template v-if="contract1 >= 4">
        <input type="radio" name="result" value="-10" id="result_10" v-model="result"><label for="result_10">-10</label>
      </template>
      <template v-if="contract1 >= 3">
        <input type="radio" name="result" value="-9" id="result_9" v-model="result"><label for="result_9">-9</label>
      </template>
      <template v-if="contract1 >= 2">
        <input type="radio" name="result" value="-8" id="result_8" v-model="result"><label for="result_8">-8</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-7" id="result_7" v-model="result"><label for="result_7">-7</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-6" id="result_6" v-model="result"><label for="result_6">-6</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-5" id="result_5" v-model="result"><label for="result_5">-5</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-4" id="result_4" v-model="result"><label for="result_4">-4</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-3" id="result_3" v-model="result"><label for="result_3">-3</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-2" id="result_2" v-model="result"><label for="result_2">-2</label>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="-1" id="result_1" v-model="result"><label for="result_1">-1</label><br>
      </template>
      <template v-if="contract1 >= 1">
        <input type="radio" name="result" value="BIEN" id="result_0" v-model="result"><label
          for="result_0">BIEN</label><br>
      </template>
      <template v-if="contract1 <= 6">
        <input type="radio" name="result" value="1" id="result1" v-model="result"><label for="result1">+1</label>
      </template>
      <template v-if="contract1 <= 5">
        <input type="radio" name="result" value="2" id="result2" v-model="result"><label for="result2">+2</label>
      </template>
      <template v-if="contract1 <= 4">
        <input type="radio" name="result" value="3" id="result3" v-model="result"><label for="result3">+3</label>
      </template>
      <template v-if="contract1 <= 3">
        <input type="radio" name="result" value="4" id="result4" v-model="result"><label for="result4">+4</label>
      </template>
      <template v-if="contract1 <= 2">
        <input type="radio" name="result" value="5" id="result5" v-model="result"><label for="result5">+5</label>
      </template>
      <template v-if="contract1 <= 1">
        <input type="radio" name="result" value="6" id="result6" v-model="result"><label for="result6">+6</label>
      </template>
      <hr>
      <button>
        Sonucu Kaydet
      </button>
    </form>
  </main>
</template>

<style scoped></style>