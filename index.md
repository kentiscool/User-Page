## My User Page

Hi I'm Kent Jonathan Utomo. As of 2021, I am in my junior year. 

<img src="https://www.kencanapp.com/_next/image?url=%2Fprofile%2Fkent.jpg&w=1920&q=75" width="100" height="100">

Stuff I'm working on:
 
[Link](https://www.kencanapp.com/)  <img src="https://www.kencanapp.com/_next/image?url=%2FKencan_header_logo.svg&w=1920&q=75" width="100" height="100">

Languages:


Favorite piece of code recently for contact tracing (Unfortunately its too slow for production but it was fun)

	func(this *DeviceInfoUseCase) TraceContact() ([][]int64, error) {
		fmt.Println("trace contact DeviceInfoUseCase")
		userList, partitionList, err := this.userLocationPostgres.GetClusters()
		if err != nil {
			fmt.Println(err.Error())
			return err
		}
		set := UniquePairsSet()
		var contactClusters [][]*_user.UserLocationInterval
		var wg sync.WaitGroup

		wg.Add(len(partitionList)-1)
		for i := 1; i < len(partitionList); i++ {
			go func(i int) {
				overlappingIntervals, err := this.getOverlappingInterval(userList[i-1 : partitionList[i]])
				if err != nil {
					return nil, err
				}
				contactClusters = append(contactClusters, overlappingIntervals...)
				wg.Done()
			} (i)
		}

		wg.Wait()


		for _, clstr := range intervalList {
			for _, interval := range clstr[1:] {
				if (clstr[0].UserID != interval.UserID) {
					set.InsertPair(clstr[0].UserID, interval.UserID)
				}
			}
		}

		return set.GetAllUniquePairs()
	}

	Local functions
	func(this *DeviceInfoUseCase) GetOverlappingInterval(locationList []*_user.UserLocation, set &) ([][]*_user.UserLocationInterval, error) {
		intervalList, err := this.userLocationPostgres.GetIntervals(locationList)
		if err != nil {
			return nil, err
		}

		var intervalClusters [][]*_user.UserLocation
		m = make(map[int64]int)

		for idx, loc := range intervalList {
			if (loc.IsStart) {
				m[loc.UserLocationID] = idx
			} else {
				if (m[loc.UserLocationID] != i-1) {
					intervalClusters = append(intervalClusters, intervalList[m[loc.UserLocationID] : idx + 1])
				}
			}
		}

		return intervalClusters, nil
	}

	type UniquePairsSet struct {
		Map map[int64]int64
	}

	func UniquePairsSet() *UniquePairsSet {
		return &UniquePairsSet{
			Map: make(map[int64]int)
		}
	}

	func(this *UniquePairsSet) InsertPair(a int64, b int64) {
		if (a < b) {
			_, ok := this.Map[a]
			if (ok) {
				return 
			}
			this.Map[a] = b
		} else {
			_, ok := this.Map[b]
			if (ok) {
				return
			} 
			this.Map[b] = a
		}
	}

	func(this *UniquePairsSet) GetAllUniquePairs() [][]int64{
		var pairs [][]int64
		for k, v := range this.Map {
			pairs = append(pairs, []int{k,v})
		}
		return pairs
	}

	func(this *UniquePairsSet) Clear() {
		Map = make(map[int64]int64)
	}

```markdown
