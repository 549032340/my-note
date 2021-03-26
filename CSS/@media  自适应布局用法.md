### @media  自适应布局用法

#### 顺序


        在css中@media (min-width: 768px)表示最小是768也就是>=768；
        @media (min-width: 768px){ //>=768的设备 }
        @media (min-width: 992px){ //>=992的设备 }
        @media (min-width: 1200){ //>=1200的设备 }
        注意下顺序，如果你把@media (min-width: 768px)写在了下面那么很悲剧，
        @media (min-width: 1200){ //>=1200的设备 }
        @media (min-width: 992px){ //>=992的设备 }
        @media (min-width: 768px){ //>=768的设备 }
        因为如果是1440,由于1440>768那么你的1200就会失效。
        所以我们用min-width时，小的放上面大的在下面，同理如果是用max-width那么就是大的在上面，小的在下面
        @media (max-width: 1199){ //<=1199的设备 }
        @media (max-width: 991px){ //<=991的设备 }
        @media (max-width: 767px){ //<=768的设备 }


