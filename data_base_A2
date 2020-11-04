import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.sql.*;
import java.util.Scanner;


public class CS3380A2Q5 {
	static Connection connection;

	public static void main(String[] args) throws Exception {

		// startup sequence
		MyDatabase db = new MyDatabase();
		doStuff(db);
		System.out.println("Exiting...");
	}

	public static void doStuff(MyDatabase db) {

		String name = "Snowball Grottobow";
		String link = "bz4bnJ77um";
		try {
			Scanner sc = new Scanner(System.in);
			System.out.println("Gimme an Elf name: ");
			String maybeName = sc.nextLine();
			System.out.println("Gimme a CheerTube link");
			String maybeLink = sc.nextLine();

			if (maybeName.length() > 0)
				name = maybeName;
			if (maybeLink.length() > 0)
				link = maybeLink;
			sc.close();
		} catch (Exception e) {
			System.out.println("Using defaults, loser.");
		}
		//Q1
		db.getAccountForElfName(name);
//		//Q2
		db.getBillsForElfName(name);
//		//Q3
		db.getVideoInformationForElfLink(link);
//		//Q4
		db.getNumberOfViewsForElfName(name);
//		//Q5
		db.getVidesViewsIsCreator();
//		//Q6
		db.getVidesViewsIsNotCreator();
		//Q7
		db.getMostMinutes();

	}


}

class MyDatabase {
	private Connection connection;
	private final String accountsTXT = "F:\\javaWorkSpace\\CS3380A2\\src\\accounts.txt";
	private final String videosTXT = "F:\\javaWorkSpace\\CS3380A2\\src\\videos.txt";
	private final String viewsTXT = "F:\\javaWorkSpace\\CS3380A2\\src\\views.txt";

	public MyDatabase() {
		try {
			Class.forName("org.hsqldb.jdbcDriver");
			// creates an in-memory database
			connection = DriverManager.getConnection("jdbc:hsqldb:mem:mymemdb", "SA", "");

			createTablesAccounts();
			readInAccountData();
			readInVideosData();
			readInViewData();
		} catch (ClassNotFoundException e) {
			e.printStackTrace(System.out);
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}


	private void createTablesAccounts() {
		// To be completed

		try {


			String accounts = "create table accounts( " +
					" accountID integer," +
					" billID integer," +
					" billingAddress VARCHAR(100)," +
					" amount VARCHAR(100)" +
					")";

			String createVideos =
					"create table videos("
							+ " creatorName varchar(100),"
							+ " videoName varchar(100),"
							+ " link varchar(100), "
							+ " duration integer)";

			String createViews =
					"create table views("
							+ " accountID integer,"
							+ " viewerName varchar(100),"
							+ " link varchar(100), "
							+ " whenTime integer)";
			connection.createStatement().executeUpdate(accounts);
			connection.createStatement().executeUpdate(createVideos);
			connection.createStatement().executeUpdate(createViews);

		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	private void createTablesVideos() {
		// To be completed
		String createVideos =
				"create table videos("
						+ " creatorName varchar(100),"
						+ " videoName varchar(100),"
						+ " link varchar(100), "
						+ " duration integer)";
		try {
			connection.createStatement().executeUpdate(createVideos);
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	private void createTablesViews() {
		// To be completed
		String createViews =
				"create table views("
						+ " accountID integer,"
						+ " viewerName varchar(100),"
						+ " link varchar(100), "
						+ " whenTime integer)";
		try {
			connection.createStatement().executeUpdate(createViews);
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getAccountForElfName(String elfName) {
		/*
		 * To be CORRECTED and completed. Just an example of how this can work. You will have to add more tables to the FROM statement
		 */
		System.out.println("Q1 - account for " + elfName);
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"Select * from views where viewerName=? limit 1;"
			);
			pstmt.setString(1, elfName);

			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				int aID = resultSet.getInt("accountID");
				System.out.println(elfName + " is associated with account " + aID);
			}

			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getBillsForElfName(String elfName) {
		System.out.println("Q2 - Bills for " + elfName);
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"Select billID,amount from accounts where accountID " +
							"in (Select accountID from views where viewerName=? )"
			);
			pstmt.setString(1, elfName);

			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				int billId = resultSet.getInt("billId");
				int amount = resultSet.getInt("amount");

				System.out.println(elfName + " has has bill " + billId + " which is for " + amount + "c");
			}

			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getVideoInformationForElfLink(String link) {
		System.out.println("Q3 - views for video with link " + link);
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"Select video.videoName as videoName,count(1) as number from views view" +
							" left join videos video" +
							" on view.link = video.link" +
							" where view.link=? group by view.link,video.videoName;"
			);
			pstmt.setString(1, link);
			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				int number = resultSet.getInt("number");
				String videoName = resultSet.getString("videoName");
				System.out.println(videoName + " /" + link + " has " + number + " views");
			}
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getNumberOfViewsForElfName(String elfName) {
		System.out.println("Q4 - videos for " + elfName + "'s and number of views");
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"Select video.creatorName as creatorName," +
							"video.videoName as videoName," +
							"count(1) as number " +
							"from videos video " +
							"left join views view " +
							"on video.link = view.link " +
							"where video.creatorName = ?" +
							"group by video.creatorName,video.videoName,video.link;"
			);
			pstmt.setString(1, elfName);

			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				String creatorName = resultSet.getString("creatorName");
				String videoName = resultSet.getString("videoName");
				int number = resultSet.getInt("number");

				System.out.println(creatorName + "'s video " + videoName + " has " + number + " views");
			}
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getVidesViewsIsCreator() {
		System.out.println("Q5 - views of videos with no other views than the creator");
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"SELECT video.videoName," +
							" COUNT(distinct case when view.viewerName !=video.creatorName then view.viewerName else null end) as number " +
							" from videos video" +
							" left join views view" +
							" on video.link = view.link" +
							" group by video.creatorName,video.videoName" +
							" having COUNT(distinct case when view.viewerName !=video.creatorName then view.viewerName else null end) =0"
			);
			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				String videoName = resultSet.getString("videoName");

				System.out.println("Video " + videoName + " has not order views");
			}
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}


	public void getVidesViewsIsNotCreator() {
		System.out.println("Q6 - viewers who are the only viewers of a video that is not the creator;");
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"SELECT videoName,count(DISTINct case when viewerName !=creatorName then viewerName else null end) as count " +
							" from videos a" +
							" left join views b" +
							" on a.link = b.link" +
							" group by videoName" +
							" having count(DISTINct case when viewerName !=creatorName then viewerName else null end) =1"
			);

			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				String videoName = resultSet.getString("videoName");
				System.out.println("Video " + videoName + " has no other views other than Winter Morningsong");
			}
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	public void getMostMinutes() {
		System.out.println("Q7 - Users with the most minutes views");
		try {
			PreparedStatement pstmt = connection.prepareStatement(
					"Select video.videoName,(count(viewerName)*duration) as minutes from videos video " +
							"left join views view " +
							"on video.link = view.link " +
							"group by video.videoName,video.duration " +
							"order by (count(viewerName)*duration) desc limit 5"
			);

			ResultSet resultSet = pstmt.executeQuery();

			while (resultSet.next()) {
				// at least 1 row (hopefully one row!) exists. Get the ID
				String videoName = resultSet.getString("videoName");
				int minutes = resultSet.getInt("minutes");
				System.out.println("Video " + videoName + " has total time " + minutes + " minutes");

			}
			pstmt.close();
		} catch (SQLException e) {
			e.printStackTrace(System.out);
		}
	}

	private void readInAccountData() {
		BufferedReader in = null;
		try {
			in = new BufferedReader((new FileReader(accountsTXT)));
			in.readLine();
			String line = in.readLine();
			while (line != null) {
				String[] parts = line.split(",");
				if (parts.length >= 4)
					makeAccount(parts[0], parts[1], parts[2], parts[3]);
				line = in.readLine();
			}
			in.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	private void readInVideosData() {
		BufferedReader in = null;
		try {
			in = new BufferedReader((new FileReader(videosTXT)));
			in.readLine();
			String line = in.readLine();
			while (line != null) {
				String[] parts = line.split(",");
				if (parts.length >= 3)
					makeVideos(parts[0], parts[1], parts[2], parts[3]);
				line = in.readLine();
			}
			in.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	private void readInViewData() {
		BufferedReader in = null;
		try {
			in = new BufferedReader((new FileReader(viewsTXT)));
			in.readLine();
			String line = in.readLine();
			while (line != null) {
				// split naively on commas
				// good enough for this dataset!
				String[] parts = line.split(",");
				if (parts.length >= 3)
					makeViews(parts[0], parts[1], parts[2], parts[3]);
				// get next line
				line = in.readLine();
			}
			in.close();
		} catch (IOException e) {
			e.printStackTrace();
		}


	}

	private void makeAccount(String accountID, String billID, String billingAddress, String amount) {
		try {
			PreparedStatement addAccount = connection.prepareStatement(
					"insert into accounts (accountID,billID,billingAddress,amount) values (?,?,?,?);"
			);
			addAccount.setInt(1, Integer.parseInt(accountID));
			addAccount.setInt(2, Integer.parseInt(billID));
			addAccount.setString(3, billingAddress);
			addAccount.setString(4, amount);
			addAccount.execute();
			addAccount.close();
		} catch (SQLException e) {
			System.out.println("Error in " + accountID + " " + billingAddress);
			e.printStackTrace(System.out);
		}
	}

	private void makeVideos(String creatorName, String videoName, String link, String duration) {
		try {
			PreparedStatement addVideos = connection.prepareStatement(
					"insert into videos (creatorName,videoName, link,duration) values (?, ?,?,?);"
			);

			addVideos.setString(1, creatorName);
			addVideos.setString(2, videoName);
			addVideos.setString(3, link);
			addVideos.setInt(4, Integer.parseInt(duration));
			addVideos.execute();
			addVideos.close();
		} catch (SQLException e) {
			System.out.println("Error in " + creatorName + " " + videoName);
			e.printStackTrace(System.out);
		}
	}

	private void makeViews(String accountID, String viewerName, String link, String when) {
		try {
			PreparedStatement addViews = connection.prepareStatement(
					"insert into views(accountID,viewerName, link,whenTime) values (?, ?,?,?);"
			);
			addViews.setInt(1, Integer.parseInt(accountID));
			addViews.setString(2, viewerName);
			addViews.setString(3, link);
			addViews.setLong(4, Long.valueOf(when));
			addViews.execute();
			addViews.close();
		} catch (SQLException e) {
			System.out.println("Error in " + accountID + " " + viewerName);
			e.printStackTrace(System.out);
		}
	}
}
