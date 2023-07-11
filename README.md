# Matri-dashboard-Problem
#include<bits/stdc++.h>
using namespace std;

struct MaintainMedian{
    
    multiset<int> smaller,larger;
    
    void rebalance(){
        if(larger.size()>smaller.size()){
            int x = *larger.begin();
            larger.erase(larger.find(x));
            smaller.insert(x);
        }
        else if(smaller.size()>larger.size()+1){
            int x = *smaller.rbegin();
            smaller.erase(smaller.find(x));
            larger.insert(x);
        }
    }
    void add(int x){
        // insert as sorted criteria
        if(smaller.empty()){
            smaller.insert(x);
        }else if( x <= *smaller.rbegin()){
            smaller.insert(x);
        }
        else{
            larger.insert(x);   
        }
        
        rebalance();
    }
    
    void erase(int x){
        if(smaller.find(x)!=smaller.end()){
            smaller.erase(smaller.find(x));   
        }else if(larger.find(x)!=larger.end()){
            larger.erase(larger.find(x));   
        }
        
        rebalance();
    }
    double getMedian(){
        if(smaller.size()==larger.size()){
            int x = *smaller.rbegin();
            int y = *larger.begin();
            return (x+y)/2.0;
        }else{
            return *smaller.rbegin();
        }
    }
};

struct MaintainMode{
    map<int,int> freq;
    multiset<pair<int,int>> mt;
    
    void add(int x){
        if(freq.find(x)!=freq.end())
            mt.erase(mt.find(make_pair(freq[x],x)));
        freq[x]++;
        mt.insert(make_pair(freq[x],x));
    }
    void erase(int x){
        mt.erase(mt.find(make_pair(freq[x],x)));
        freq[x]--;
        mt.insert(make_pair(freq[x],x));
    }
    int getMode(){
        int modefreq = mt.rbegin()->first;
        auto it = mt.lower_bound(make_pair(modefreq,-1e9));
        return it->second;
    }
};

struct Dashboard{
    int sum = 0;
    int sumsq = 0;
    int count = 0;
    MaintainMedian myMedian;
    MaintainMode myMode;
    
    void add(int x){
        sum += x;
        sumsq += x*x;
        count++;
        myMedian.add(x);
        myMode.add(x);
    }
    void erase(int x){
        sum -= x;
        sumsq -= x*x;
        count--;
        myMedian.erase(x);
        myMode.erase(x);
    }
    double getMean(){
        return double(sum)/count;
    }
    double getVairance(){
        return double(sumsq)/count - getMean()*getMean();
    }
    double getMedian(){
        return myMedian.getMedian();
    }
    int getMode(){
        return myMode.getMode();
    }
};

int main(){
    Dashboard dashboard;
    dashboard.add(3);
    dashboard.add(5);
    dashboard.add(7);
    dashboard.add(3);
    cout<<dashboard.getMean()<<" ";
    cout<<dashboard.getVairance()<<" ";
    cout<<dashboard.getMedian()<<" ";
    cout<<dashboard.getMode()<<" ";
}
