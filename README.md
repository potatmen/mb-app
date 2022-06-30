using namespace std;
#include <bits/stdc++.h>
#include <cell.h>

class Field{
    vector<vector<Cell>> grid;
    int n;
    int m;
    Field(int n,int m,set<pair<int,int>> p);
    Field live();
    Field with(int x, int y, Cell c, Field initial);
    void print();
}



using namespace std;
#include <bits/stdc++.h>
#include <field.h>
class Cell{
    bool state;
    Cell(bool state);
    Cell live(int x,int y,Field f);
}


#include<boost/program_options.hpp>
namespace po = boost::program_options;
class Parse{
    po::options_description desc("Allowed options");
    po::variables_map vm;
    int n;
    int m;
    set<pair<int,int>> points;
    Parse(int ac, char *av[]);
    bool is_set(string s);
    bool is_set(int x,int y);
}



#include "../include/field.h"
#include "../include/parse.h"
#include <unistd.h>
void sleep(int x){
  unsigned int microsecond = time * x;
  usleep(microsecond);
}

int main(int ac, char *av[]){
    Parse p = Parse(ac,av);
    Field f = Field(p.n, p.m, p.points);
    if(p.is_set("batch")){
        for(int i = 0; i < p.vm["batch"] + 1; i++){
            f.print();
            cout << "This iteration number " << i << endl;
            sleep(p.vm["sleep"]);
            f = f.live();
        }
    }
    else{
        while(true){
            f.print();
            cout << "If you want to continue playing press \"b\", else \"q\".";
            string s;
            cin >> s;
            if(s == "n"){
               f = f.live();
            }
            else{
                break;
            }                      
        }
    }
}
