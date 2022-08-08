// Shortest Seek Time First.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include <iostream>
#include <list>
#include <set>
#include <algorithm>
using namespace std;

#define Tracks 5
#define Sectors 5

struct req
{
    int track;
    int sector;
};

bool track_compare(const req& a, const req& b)
{
    return a.track < b.track;
}

bool sector_compare(const req& a, const req& b)
{
    if (a.track == b.track)
        return a.sector < b.sector;
    else
        return 0;
}

int main()
{
    list<req> requests;
    list<req>::iterator it = requests.begin();

    int n = 0, x = -1, y = -1;
    int seekTime = 5, searchTime = 1, transferTime = 1, totalSeek = 0, totalSearch = 0, totalTransfer = 0;
    int track = 0, sector = 0;
    cout << "Enter the number of requests:";
    cin >> n;
    cout << endl;
    cout << "Enter the order of requests e.g: 4 7" << endl;
    cout << "                                 3 1" << endl;
    cout << "                                 ..." << endl;

    for (int i = 0; i < n; i++)
    {
        cin >> x;
        cin >> y;
        requests.insert(it,{x,y});
    }
    it = requests.begin();
    
    //Sorting the list
    requests.sort(track_compare);
    requests.sort(sector_compare);

    it = requests.begin();
    for (int i = 0; i < requests.size(); i++)
    {
        req c = *it;
        
        totalSeek += (abs(c.track - track)) * seekTime;
        track = c.track;

        if (c.sector > sector)
            totalSearch += ((c.sector - sector) * searchTime);
        else if (sector > c.sector)
            totalSearch += ((Sectors - sector) + c.sector) * searchTime;

        sector = c.sector + 1;
        if (sector == Sectors)
            sector = 0;

        it++;
    }
    totalTransfer = n * transferTime;

    cout << "Total seek     = " << totalSeek << " ms" << endl;
    cout << "Total search   = " << totalSearch << " ms" << endl;
    cout << "Total transfer = " << totalTransfer << " ms" << endl;
    cout << "Total Time     = " << totalSeek + totalSearch + totalTransfer << " ms" << endl;

}





