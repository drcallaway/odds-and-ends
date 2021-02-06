# Interview Questions

Every engineer is occasionally presented some interesting programming problems during an interview. Here are my solutions to some of the interview questions I've been asked in the past.

## Simple Run-Length Encoding Algorithm
```java
/**
 * This is my solution for an interview question that asked me to write an app that would run-length encode a string
 * consisting of a series of zeros and ones.
 *
 * @author Dustin R. Callaway
 * @version July 8, 2012
 */
public class RunLengthEncoding
{
    public static void main(String[] args)
    {
        String original = "000001100011111111100000101001000011";
        int counter = 0;
        char last = ' ';
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < original.length(); i++)
        {
            char current = original.charAt(i);

            if (last == ' ' || last == current)
            {
                counter++;
            }
            else
            {
                sb.append(counter).append(last).append("-");
                counter = 1;
            }

            last = current;
        }

        sb.append(counter).append(last);

        System.out.println(sb.toString());
    }
}
```

## Find Path Algorithm
```java
import java.util.ArrayList;
import java.util.List;

/**
 * This is my solution to an interview question I was asked to find any path between two subway stations.
 * The stations can be represented in a non-directed cyclic graph that looks like this:
 *
 * A--C--E--F
 * |  | /
 * B--D
 *
 * @author Dustin R. Callaway
 * @version July 7, 2012
 */
public class Subway
{
    public static void main(String[] args)
    {
        Station a = new Station("A");
        Station b = new Station("B");
        Station c = new Station("C");
        Station d = new Station("D");
        Station e = new Station("E");
        Station f = new Station("F");

        //construct the graph (each station points to all connected stations)
        a.addStation(b);
        a.addStation(c);

        b.addStation(a);
        b.addStation(d);

        c.addStation(a);
        c.addStation(d);
        c.addStation(e);

        d.addStation(b);
        d.addStation(c);
        d.addStation(e);

        e.addStation(c);
        e.addStation(d);
        e.addStation(f);

        f.addStation(e);

        Subway subway = new Subway();

        List<Station> path = new ArrayList<Station>();

        if (subway.findPath(b, f, path)) //specify start and end stations here
        {
            System.out.print("Path is: ");

            for (int i = 0; i < path.size(); i++)
            {
                if (i > 0)
                {
                    System.out.print(" to ");
                }

                System.out.print(path.get(i).getName());
            }

            System.out.println();
        }
        else
        {
            System.out.println("Path not found");
        }
    }

    private boolean findPath(Station start, Station end, List<Station> path)
    {
        boolean pathFound = false;

        boolean stationAlreadyVisited = path.contains(start);

        path.add(start);

        if (!stationAlreadyVisited)
        {
            for (Station station : start.getConnectedStations())
            {
                if (station == end)
                {
                    path.add(station); //reached destination -- add final station to path
                    pathFound = true;
                    break;
                }
                else if (findPath(station, end, path))
                {
                    pathFound = true;
                    break;
                }
                else
                {
                    path.remove(path.size()-1); //back out of bad path
                }
            }
        }

        return pathFound;
    }

    private static class Station
    {
        private String name;
        private List<Station> connectedStations = new ArrayList<Station>();

        public Station(String name)
        {
            this.name = name;
        }

        public String getName()
        {
            return name;
        }

        public List<Station> getConnectedStations()
        {
            return connectedStations;
        }

        public void addStation(Station station)
        {
            connectedStations.add(station);
        }
    }
}
```

## Find Shortest Path Algorithm
```java
import java.util.ArrayList;
import java.util.List;

/**
 * Taking the previous example one step further, here is my solution for finding the shortest path
 * (fewest hops) between subway stations. The stations can be represented in a non-directed cyclic
 * graph that looks like this:
 *
 * A--C--E--F
 * |  | /
 * B--D
 *
 * @author Dustin R. Callaway
 * @version July 7, 2012
 */
public class Subway
{
    private List<Station> shortestPath;

    public static void main(String[] args)
    {
        Station a = new Station("A");
        Station b = new Station("B");
        Station c = new Station("C");
        Station d = new Station("D");
        Station e = new Station("E");
        Station f = new Station("F");

        //construct the graph (each station points to all connected stations)
        a.addStation(b);
        a.addStation(c);

        b.addStation(a);
        b.addStation(d);

        c.addStation(a);
        c.addStation(d);
        c.addStation(e);

        d.addStation(b);
        d.addStation(c);
        d.addStation(e);

        e.addStation(c);
        e.addStation(d);
        e.addStation(f);

        f.addStation(e);

        Subway subway = new Subway();

        List<Station> shortestPath = subway.findPath(a, f, null); //specify start and end stations here

        if (shortestPath != null)
        {
            System.out.print("Shortest path is: ");

            for (int i = 0; i < shortestPath.size(); i++)
            {
                if (i > 0)
                {
                    System.out.print(" to ");
                }

                System.out.print(shortestPath.get(i).getName());
            }

            System.out.println();
        }
        else
        {
            System.out.println("Path not found");
        }
    }

    private List<Station> findPath(Station start, Station end, List<Station> path)
    {
        if (path == null)
        {
            path = new ArrayList<Station>();
        }

        boolean stationAlreadyVisited = path.contains(start);

        path.add(start);

        if (!stationAlreadyVisited && (shortestPath == null || path.size() < shortestPath.size()))
        {
            for (Station station : start.getConnectedStations())
            {
                if (station == end)
                {
                    shortestPath = new ArrayList<Station>(path); //we reached the end so save path
                    shortestPath.add(station);
                    break;
                }
                else
                {
                    findPath(station, end, path); //recurse deeper
                    path.remove(path.size() - 1); //remove nodes that have been explored
                }
            }
        }

        return shortestPath;
    }

    private static class Station
    {
        private String name;
        private List<Station> connectedStations = new ArrayList<Station>();

        public Station(String name)
        {
            this.name = name;
        }

        public String getName()
        {
            return name;
        }

        public List<Station> getConnectedStations()
        {
            return connectedStations;
        }

        public void addStation(Station station)
        {
            connectedStations.add(station);
        }
    }
}
```
